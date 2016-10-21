---
title: 使用JRebel实现热部署
date: 2016-03-18 09:48:00
updated: 2016-03-18 09:48:00
comments: true
categories:
  - program
  - java
tags:
  - java
  - jrebel
---

> 情景：
> 1. xx-biz里的XXServiceImpl有问题，改；
> 2. `mvn install`，等；
> 3. 重启应用，等；
> 4. 李大爷说还是有问题，再改，回到步骤1；

<!-- more -->

以上情景相信大家并不陌生。然后，不知道大家有没有统计过每天浪费在【编译】+【重启应用】的时间有多少呢？浪费时间的同时也十分影响工作情绪，也容易打断思路。

为此，本人尝试用了一下`JRebel`，分享一下。

1. 安装JRebel插件（这里以Intelij IDEA为例）；
{% asset_img p1.png %}
2. [UNO](http://idea.lanyus.com/)；
3. 打开左侧的JRebel标签，将需要热部署的Maven模块打勾（这里以supplier-web为例），然后IDE会自动在打勾模块下的`src/main/resources`下生成`rebel.xml`（***建议在`.gitignore`文件中添加`rebel.xml`，防止提交到git仓库***）；
{% asset_img p2.png %}
4. 打开`supplier-web-publish`下的`pom.xml`，将`jetty`插件的`scanIntervalSeconds`的值设为**0**，关闭jetty的自动重启；
{% asset_img p3.png %}
5. 点击右上角的`Debug with JRebel`，以debug模式启动项目；
{% asset_img p4.png %}
6. 在项目运行的状态下，尝试修改Java代码，然后重新编译该文件（右键，Compile '…….java'）；
7. 如果控制台出现以下信息，表示JRebel已经能正常运作了；
{% asset_img p5.png %}
8. Fly me to the moon and let me play among the stars.

> **Save time, save life.**
