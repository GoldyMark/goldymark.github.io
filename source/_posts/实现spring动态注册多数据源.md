---
title: 实现Spring动态注册多数据源
comments: true
tags:
  - java
  - spring
  - 动态多数据源
categories:
  - program
  - java
date: 2018-01-17 00:04:29
updated: 2018-01-17 00:04:29
---

最近在做SaaS应用，数据库采用了单实例多schema的架构（详见参考资料1），每个租户有一个独立的schema，同时整个数据源有一个共享的schema，因此需要解决动态增删、切换数据源的问题。

在网上搜了很多文章后，很多都是讲主从数据源配置，或都是在应用启动前已经确定好数据源配置的，甚少讲在不停机的情况如何动态加载数据源，所以写下这篇文章，以供参考。

## 使用到的技术

+ Java8
+ Spring + SpringMVC + MyBatis
+ Druid连接池
+ Lombok
+ （以上技术并不影响思路实现，只是为了方便浏览以下代码片段）

## 思路

当一个请求进来的时候，判断当前用户所属租户，并根据租户信息切换至相应数据源，然后进行后续的业务操作。

## 代码实现

## TenantConfigEntity（租户信息）

```java
@EqualsAndHashCode(callSuper = false)
@Data
@FieldDefaults(level = AccessLevel.PRIVATE)
public class TenantConfigEntity {
    /**
     * 租户id
     **/
    Integer tenantId;
    /**
     * 租户名称
     **/
    String tenantName;
    /**
     * 租户名称key
     **/
    String tenantKey;
    /**
     * 数据库url
     **/
    String dbUrl;
    /**
     * 数据库用户名
     **/
    String dbUser;
    /**
     * 数据库密码
     **/
    String dbPassword;
    /**
     * 数据库public_key
     **/
    String dbPublicKey;
}
```

##  DataSourceUtil（辅助工具类，非必要）

```java
public class DataSourceUtil {

    private static final String DATA_SOURCE_BEAN_KEY_SUFFIX = "_data_source";

    private static final String JDBC_URL_ARGS = "?useUnicode=true&characterEncoding=UTF-8&useOldAliasMetadataBehavior=true&zeroDateTimeBehavior=convertToNull";

    private static final String CONNECTION_PROPERTIES = "config.decrypt=true;config.decrypt.key=";
    /**
     * 拼接数据源的spring bean key
     */
    public static String getDataSourceBeanKey(String tenantKey) {
        if (!StringUtils.hasText(tenantKey)) {
            return null;
        }
        return tenantKey + DATA_SOURCE_BEAN_KEY_SUFFIX;
    }
    /**
     * 拼接完整的JDBC URL
     */
    public static String getJDBCUrl(String baseUrl) {
        if (!StringUtils.hasText(baseUrl)) {
            return null;
        }
        return baseUrl + JDBC_URL_ARGS;
    }
    /**
     * 拼接完整的Druid连接属性
     */
    public static String getConnectionProperties(String publicKey) {
        if (!StringUtils.hasText(publicKey)) {
            return null;
        }
        return CONNECTION_PROPERTIES + publicKey;
    }
}
```

### DataSourceContextHolder

使用**ThreadLocal**保存当前线程的数据源key name，并实现set、get、clear方法；

```java
public class DataSourceContextHolder {

    private static final ThreadLocal<String> dataSourceKey = new InheritableThreadLocal<>();

    public static void setDataSourceKey(String tenantKey) {
        dataSourceKey.set(tenantKey);
    }

    public static String getDataSourceKey() {
        return dataSourceKey.get();
    }

    public static void clearDataSourceKey() {
        dataSourceKey.remove();
    }
}
```

###  DynamicDataSource（重点）

继承**AbstractRoutingDataSource**（建议阅读其源码，了解动态切换数据源的过程），实现动态选择数据源；

```java
public class DynamicDataSource extends AbstractRoutingDataSource {

    @Autowired
    private ApplicationContext applicationContext;

    @Lazy
    @Autowired
    private DynamicDataSourceSummoner summoner;

    @Lazy
    @Autowired
    private TenantConfigDAO tenantConfigDAO;

    @Override
    protected String determineCurrentLookupKey() {
        String tenantKey = DataSourceContextHolder.getDataSourceKey();
        return DataSourceUtil.getDataSourceBeanKey(tenantKey);
    }

    @Override
    protected DataSource determineTargetDataSource() {
        String tenantKey = DataSourceContextHolder.getDataSourceKey();
        String beanKey = DataSourceUtil.getDataSourceBeanKey(tenantKey);
        if (!StringUtils.hasText(tenantKey) || applicationContext.containsBean(beanKey)) {
            return super.determineTargetDataSource();
        }
        if (tenantConfigDAO.exist(tenantKey)) {
            summoner.registerDynamicDataSources();
        }
        return super.determineTargetDataSource();
    }
}
```

## DynamicDataSourceSummoner（重点中的重点）

从数据库加载数据源信息，并动态组装和注册spring bean，

```java
@Slf4j
@Component
public class DynamicDataSourceSummoner implements ApplicationListener<ContextRefreshedEvent> {

    // 跟spring-data-source.xml的默认数据源id保持一致
    private static final String DEFAULT_DATA_SOURCE_BEAN_KEY = "defaultDataSource";

    @Autowired
    private ConfigurableApplicationContext applicationContext;
    @Autowired
    private DynamicDataSource dynamicDataSource;
    @Autowired
    private TenantConfigDAO tenantConfigDAO;

    private static boolean loaded = false;
    /**
     * Spring加载完成后执行
     */
    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        // 防止重复执行
        if (!loaded) {
            loaded = true;
            try {
                registerDynamicDataSources();
            } catch (Exception e) {
                log.error("数据源初始化失败, Exception:", e);
            }
        }
    }
    /**
     * 从数据库读取租户的DB配置,并动态注入Spring容器
     */
    public void registerDynamicDataSources() {
        // 获取所有租户的DB配置
        List<TenantConfigEntity> tenantConfigEntities = tenantConfigDAO.listAll();
        if (CollectionUtils.isEmpty(tenantConfigEntities)) {
            throw new IllegalStateException("应用程序初始化失败,请先配置数据源");
        }
        // 把数据源bean注册到容器中
        addDataSourceBeans(tenantConfigEntities);
    }
    /**
     * 根据DataSource创建bean并注册到容器中
     */
    private void addDataSourceBeans(List<TenantConfigEntity> tenantConfigEntities) {
        Map<Object, Object> targetDataSources = Maps.newLinkedHashMap();
        DefaultListableBeanFactory beanFactory = (DefaultListableBeanFactory) applicationContext.getAutowireCapableBeanFactory();
        for (TenantConfigEntity entity : tenantConfigEntities) {
            String beanKey = DataSourceUtil.getDataSourceBeanKey(entity.getTenantKey());
            // 如果该数据源已经在spring里面注册过,则不重新注册
            if (applicationContext.containsBean(beanKey)) {
                DruidDataSource existsDataSource = applicationContext.getBean(beanKey, DruidDataSource.class);
                if (isSameDataSource(existsDataSource, entity)) {
                    continue;
                }
            }
            //  组装bean
            AbstractBeanDefinition beanDefinition = getBeanDefinition(entity, beanKey);
            //  注册bean
            beanFactory.registerBeanDefinition(beanKey, beanDefinition);
            //  放入map中，注意一定是刚才创建bean对象
            targetDataSources.put(beanKey, applicationContext.getBean(beanKey));
        }
        //  将创建的map对象set到 targetDataSources；
        dynamicDataSource.setTargetDataSources(targetDataSources);
        //  必须执行此操作，才会重新初始化AbstractRoutingDataSource 中的 resolvedDataSources，也只有这样，动态切换才会起效
        dynamicDataSource.afterPropertiesSet();
    }
    /**
     * 组装数据源spring bean
     */
    private AbstractBeanDefinition getBeanDefinition(TenantConfigEntity entity, String beanKey) {
        BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(DruidDataSource.class);
        builder.getBeanDefinition().setAttribute("id", beanKey);
        // 其他配置继承defaultDataSource
        builder.setParentName(DEFAULT_DATA_SOURCE_BEAN_KEY);
        builder.setInitMethodName("init");
        builder.setDestroyMethodName("close");
        builder.addPropertyValue("name", beanKey);
        builder.addPropertyValue("url", DataSourceUtil.getJDBCUrl(entity.getDbUrl()));
        builder.addPropertyValue("username", entity.getDbUser());
        builder.addPropertyValue("password", entity.getDbPassword());
        builder.addPropertyValue("connectionProperties", DataSourceUtil.getConnectionProperties(entity.getDbPublicKey()));
        return builder.getBeanDefinition();
    }
    /**
     * 判断Spring容器里面的DataSource与数据库的DataSource信息是否一致
     * 备注:这里没有判断public_key,因为另外三个信息基本可以确定唯一了
     */
    private boolean isSameDataSource(DruidDataSource existsDataSource, TenantConfigEntity entity) {
        boolean sameUrl = Objects.equals(existsDataSource.getUrl(), DataSourceUtil.getJDBCUrl(entity.getDbUrl()));
        if (!sameUrl) {
            return false;
        }
        boolean sameUser = Objects.equals(existsDataSource.getUsername(), entity.getDbUser());
        if (!sameUser) {
            return false;
        }
        try {
            String decryptPassword = ConfigTools.decrypt(entity.getDbPublicKey(), entity.getDbPassword());
            return Objects.equals(existsDataSource.getPassword(), decryptPassword);
        } catch (Exception e) {
            log.error("数据源密码校验失败,Exception:{}", e);
            return false;
        }
    }
}
```

## spring-data-source.xml

```xml
    <!-- 引入jdbc配置文件 -->
    <context:property-placeholder location="classpath:data.properties" ignore-unresolvable="true"/>

    <!-- 公共(默认)数据源 -->
    <bean id="defaultDataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <!-- 基本属性 url、user、password -->
        <property name="url" value="${ds.jdbcUrl}" />
        <property name="username" value="${ds.user}" />
        <property name="password" value="${ds.password}" />

        <!-- 配置初始化大小、最小、最大 -->
        <property name="initialSize" value="5" />
        <property name="minIdle" value="2" />
        <property name="maxActive" value="10" />

        <!-- 配置获取连接等待超时的时间，单位是毫秒 -->
        <property name="maxWait" value="1000" />

        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="5000" />

        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="240000" />

        <property name="validationQuery" value="SELECT 1" />
        <!--单位：秒，检测连接是否有效的超时时间-->
        <property name="validationQueryTimeout" value="60" />
        <!--建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效-->
        <property name="testWhileIdle" value="true" />
        <!--申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。-->
        <property name="testOnBorrow" value="true" />
        <!--归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。-->
        <property name="testOnReturn" value="false" />

        <!--Config Filter-->
        <property name="filters" value="config" />
        <property name="connectionProperties" value="config.decrypt=true;config.decrypt.key=${ds.publickey}" />
    </bean>

    <!-- 事务管理器 -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="multipleDataSource"/>
    </bean>

    <!--多数据源-->
    <bean id="multipleDataSource" class="a.b.c.DynamicDataSource">
        <property name="defaultTargetDataSource" ref="defaultDataSource"/>
        <property name="targetDataSources">
            <map>
                <entry key="defaultDataSource" value-ref="defaultDataSource"/>
            </map>
        </property>
    </bean>

    <!-- 注解事务管理器 -->
    <!--这里的order值必须大于DynamicDataSourceAspectAdvice的order值-->
    <tx:annotation-driven transaction-manager="txManager" order="2"/>

    <!-- 创建SqlSessionFactory，同时指定数据源 -->
    <bean id="mainSqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="multipleDataSource"/>
    </bean>

    <!-- DAO接口所在包名，Spring会自动查找其下的DAO -->
    <bean id="mainSqlMapper" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="mainSqlSessionFactory"/>
        <property name="basePackage" value="a.b.c.*.dao"/>
    </bean>

    <bean id="defaultSqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="defaultDataSource"/>
    </bean>

    <bean id="defaultSqlMapper" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="defaultSqlSessionFactory"/>
        <property name="basePackage" value="a.b.c.base.dal.dao"/>
    </bean>

    <!-- 其他配置省略 -->
```

## DynamicDataSourceAspectAdvice

利用AOP自动切换数据源，仅供参考；

```java
@Slf4j
@Aspect
@Component
@Order(1) // 请注意：这里order一定要小于tx:annotation-driven的order，即先执行DynamicDataSourceAspectAdvice切面，再执行事务切面，才能获取到最终的数据源
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class DynamicDataSourceAspectAdvice {

    @Around("execution(* a.b.c.*.controller.*.*(..))")
    public Object doAround(ProceedingJoinPoint jp) throws Throwable {
        ServletRequestAttributes sra = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = sra.getRequest();
        HttpServletResponse response = sra.getResponse();
        String tenantKey = request.getHeader("tenant");
        // 前端必须传入tenant header, 否则返回400
        if (!StringUtils.hasText(tenantKey)) {
            WebUtils.toHttp(response).sendError(HttpServletResponse.SC_BAD_REQUEST);
            return null;
        }
        log.info("当前租户key:{}", tenantKey);
        DataSourceContextHolder.setDataSourceKey(tenantKey);
        Object result = jp.proceed();
        DataSourceContextHolder.clearDataSourceKey();
        return result;
    }
}
```

## 其他

另外，部分信息（例如：租户配置、省市县等共用信息）是存放在公共schema里面的，也就是说一个请求里面有可能包括查询**当前租户数据源**和**公共数据源**，建议为公共数据源单独创建一个`sqlSessionFactory`和`sqlMapper`（分别对应xml配置里的`defaultSqlSessionFactory`和`defaultSqlMapper`），或干脆把**公共数据源**的数据单独做成一个微服务，以后水平扩展也方便。

## 参考资料

1. [SaaS多租户数据隔离的三种方案](http://blog.csdn.net/yown/article/details/51288549)
2. [Spring MVC+Mybatis 多数据源配置](https://www.jianshu.com/p/fddcc1a6b2d8)
3. [spring 动态创建bean](http://blog.csdn.net/getdate/article/details/20057877)
4. [Saas Spring动态加载、编辑数据源](http://blog.csdn.net/xwq911/article/details/49147927)
