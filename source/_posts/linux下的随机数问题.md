---
title: linux下的随机数问题
comments: true
date: 2014-09-05 17:49:00
updated: 2014-09-05 17:49:00
categories:
  - program
  - java
tags:
  - java
  - linux
  - 随机数
---

### 需求
用`java`将字符串通过`DES`加密。

### 本地环境：
**操作系统：**Windows XP SP3 32bit
**JDK版本：**Sun JDK6

### 服务器环境
**操作系统：**Linux 64bit
**JDK版本：**Open JDK7

百度了一下，很多相关代码，直接[拿来就用](http://blog.csdn.net/shibenjie/article/details/5365355)，发现有一个问题：
在本地测试的时候没问题，但发布到服务器后，同一个`text`和`keyStr`，每次加密生成的字符串竟然是不同的。

<!-- more -->

最后把问题代码定位到`getKey()`方法上，代码如下：

```java
/**
 * 根据参数生成Key;
 *
 * @param keyStr
 */
private static Key getKey(String keyStr) {
    Key key = null;
    try {
        KeyGenerator keyGenerator = KeyGenerator.getInstance(ALGORITHM);
        keyGenerator.init(new SecureRandom(keyStr.getBytes()));
        key = keyGenerator.generateKey();
        keyGenerator = null;
    } catch (Exception e) {
        e.printStackTrace();
    }
    return key;
}
```

在服务器上单独测试了几次这段代码，发现每次传入同一个keyStr生成的Key值，返回的数据竟然是不同的，测试代码如下：
```java
for (int i = 0; i < 5; i++) {
  System.out.println(new String(getKey("test").getEncoded()));
}
```

为此我怀疑过是不是32位和64位、JDK6和JDK7、SunJDK和OpenJDK之间的问题，用虚拟机虚拟了几个环境测试，发现如下（OK表示没问题，FALSE表示有问题）：
1、Win7 32bit      SunJDK7                        -----------OK
2、LinuxMint 32bit OpenJDK7                 -----------FALSE
3、WinXP 32bit     SunJDK6（本地环境）           -----------OK
4、Mac 64bit       OpenJDK7                   -----------FALSE
5、SL-Linux 64bit  OpenJDK7（服务器环境）  -----------FALSE

当时就在纠结上面那3个怀疑方向，google了一番之后，好像也没人提到32位/64位的,于是怀疑JDK6和JDK7和SunJDK和OpenJDK之间的差异问题，就在上面几次测试之后，突然想起，会不会是操作系统之间的差异问题呢？！
因为本人之前认为Java是跨平台的，JVM在不同平台下对代码的解释应该是一样的（差异仅在性能之间），所以没想到是操作系统之间的差异问题。但因为这段代码涉及到随机数的生成问题，而随机数可能是跟操作系统有关系的，所以根据这方面的联想google一番之后，got it！
貌似这是一个著名的linux问题……
后来将getKey()方法里的try块修改如下：
```java
try {
    KeyGenerator keyGenerator = KeyGenerator.getInstance(ALGORITHM);
    SecureRandom random = SecureRandom.getInstance("SHA1PRNG");
    random.setSeed(keyStr.getBytes());
    keyGenerator.init(random);
    key = keyGenerator.generateKey();
    keyGenerator = null;
} catch (Exception e) {
    e.printStackTrace();
}
```

问题解决！
