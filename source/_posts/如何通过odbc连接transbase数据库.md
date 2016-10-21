---
title: 如何通过ODBC连接transbase数据库
comments: true
date: 2016-10-20 23:31:30
updated: 2016-10-20 23:31:30
tags:
  - database
  - transbase
categories:
  - program
  - database
---

最近在研究transbase数据库，发现transbase提供`JDBC`和`ODBC`两种连接驱动。前者比较简单就不叙述了，这里主要说一下如何通过`ODBC`驱动连接transbase数据库。

<!-- more -->

*hint：关于在`win7 32bit/64bit`下如何打开`ODBC`配置，请自行搜索相关知识，本文不作介绍。*

## 第一步：配置ODBC驱动

按照以下配置修改注册表：
```
[HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI\<DRIVERNAME>]
"UsageCount"=dword:00000001
"Driver"="<INSTALLATIONPATH>\\tbodbc32.dll"
"Setup"="<INSTALLATIONPATH>\\tbsodb32.dll"
"Configuration"="<INSTALLATIONPATH>\\tbwin.ini"

[HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI\ODBC Drivers] "<DRIVERNAME"="Installed"
```

**注意：**
+ 把`<DRIVERNAME>`替换成单独的驱动名称（不与现有的重复即可），例如：`transbase`；
+ 把`<INSTALLATIONPATH>`替换成`transbase`的安装目录；

## 第二步：配置DSN

```
[HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI\ODBC Data Sources]
"<DSN>"="<DRIVERNAME>"
[HKEY_LOCAL_MACHINE\ SOFTW ARE\ODBC\ODBC.INI\<DSN>]
"Driver"="<INSTALLATIONPATH>\\tbodbc32.dll"
"Description"="Electronic Part Catalogue B&H"
"Dbhost"="<SERVERNAME>"
"Dbname"="<DBNAME>"
"Option1"="No"
"Option2"="No"
```

**注意：**
+ 把`<DRIVERNAME>`替换成**第一步**中的`<DRIVERNAME>`，例如：`transbase`；
+ 把`<INSTALLATIONPATH>`替换成`transbase`的安装目录；
+ 把`<DSN>`替换成单独的数据源名称（不与现有的重复即可），例如：`EPC B&H`；
+ 把`<SERVER>`替换成`transbase`数据库的host地址；
+ 把`<DBNAME>`替换成`transbase`数据库名称；

接下来就可以按网上连接ODBC驱动的教程连接`transbase`数据库了。

> Google搜索关键词
> transbase redistribution
