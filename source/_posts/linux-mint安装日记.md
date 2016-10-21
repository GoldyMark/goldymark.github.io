---
title: Linux Mint安装日记
comments: true
date: 2012-10-23 11:49:01
updated: 2012-10-23 11:49:01
tags:
  - Linux
  - mint
categories:
  - technology
  - Linux
---

**Linux mint**
http://www.linuxmint.com/

<!-- more -->

关于Mint的简介我就不多说了，跟Ubuntu差不多吧，但感觉上没有Ubuntu花哨，这就是我选择Mint的原因。
Ubuntu有个工具叫`wubi`，`wubi`就是可以在windows下安装ubuntu的工具，不会对现有的windows系统产生任何影响（装完后的linux就是一个文件夹，想删除linux就直接删除文件夹即可）。Mint也有个类似的工具，叫`Mint4Win`。
Mint 13有4种不同桌面版本，分别是`Mate`、`Cinnamon`、`KDE`和`XFCE`。
+ `KDE`: 个人感觉KDE4比较不稳定，容易崩溃（之前装OpenSUSE12.2的时候经常崩溃，故无好感），而且也比较花哨，不怎么好用；
+ `Cinnamon`: 不怎么熟悉，试用了1、2次之后有点不习惯，所以弃用；
+ `Mate`: 基于`Gnome2`做成的桌面系统，个人比较喜欢`Gnome2`的风格，所以最后选择了`Mate`版本；
+ `XFCE`: 还不错，可惜没有自带`Mint4Win`工具，所以暂时不用（理论上`Mint4Win`也可以安装的）；

去官网的下载页面可以看出，`Mint4Win`只支持`Mate`和`Cinnamon`的64bit版本，32bit版本不支持（虽然下载的iso镜像有这个软件，但安装过程会报错），后来搜出了一篇文章介绍如何用`Mint4Win`安装32bit版本的`Mate`和`Cinnamon`（个人猜测`KDE`和`XFCE`版本也可以，但没试过）。
文章地址：http://forums.linuxmint.com/viewtopic.php?f=42&t=106922
恩，英文的……
以下操作我均在`Vmware`虚拟机进行的。
虚拟机环境如下：60G硬盘（10+10+40）【按Windows分区】，1G内存，已安装好的XP。
准备工作：下载`Mate`或`Cinnamon`的32bit镜像，用虚拟机载入镜像，打开虚拟机，从光盘运行，然后在启动菜单选择`Star Linux Mint`，进入live版Mint。
我试图用简单的语言解释这篇文章吧。
我直接从文章的**How to solve the problem**开始。
打开终端窗口，输入以下命令【注意：Linux命令区分大小写的】：
**命令：**
```shell
cd /cdrom/casper
ls -al
```
解释：你将看到`vmlinuz`和`initrd.lz`这两个文件。这两个文件会在`Mint4win`安装系统的第一阶段复制到Windows机器下。我们要修复的就是这个`initrd.lz`。回到home文件夹下，新建一个`x`文件夹（或其他名字，随意）。

**命令：**
```shell
cd /home
mkdir x
cd x
```
解释：将initrd.lz解压到x文件夹下。

**命令：**
```shell
zcat /cdrom/casper/initrd.lz | cpio -i -d //key!
cd sbin
ls -al mount.ntf*
file mount.ntf*
```
解释：后面两个命令会列出两个文件并且显示出他们是64位可执行程序，但实际上他们应该是32位可执行程序。

**命令：**
```shell
ls -al ../bin/ntfs-3g
file ../bin/ntfs-3g
```
解释：这两条命令会正确列出相同文件的32位版本，只是文件名稍稍不同。

**关键命令：**
```shell
cp ../bin/ntfs-3g mount.ntfs       //key!
cp ../bin/ntfs-3g mount.ntfs-3g （此处原文有误，已修正） //key!
file mount.ntf*
```
解释：现在我们需要按整个子文件夹的结构重新打包创建一个新的initrd.lz文件，这些命令会在home文件夹下生成一个新的initrd.lz文件。

**命令：**
```shell
cd ~/x
find . | cpio -o -H newc | gzip -9 > ../initrd.lz  //key!
cd
ls -al
```

其实简单来说，就是把`initrd.lz`解压后，将`bin`文件夹下的`ntfs-3g`替换掉`sbin`文件夹下的`mount.ntfs`和`mount.ntfs-3g`这两个文件（但文件名仍然使用原来的文件名），然后再重新打包。

至此，一个新的`initrd.lz`已经出现在`home`文件夹下了，将这个文件保存到c盘或d盘下面（在linux里面要自己挂载对应分区），文章的下面我就不翻译了，都是比较浅显的操作。
教程所说的办法其实就是在安装过程把这个`initrd.lz`文件替换掉，我觉得比较麻烦，索性就重新将`initrd.lz`替换掉iso镜像里面的同名文件算了，同时也要更新对应的md5码。
所以，最后要替换掉镜像里面两个文件：
+ `/casper下的initrd.lz`
+ `/md5sum.txt`

其中`md5sum.txt`要修改一处地方（不建议用记事本操作，我是用EditPlus的），找个软件算出新生成的`initrd.lz`的md5码，然后搜索文档里面的`initrd.lz`那一行，将前面的md5码删掉，输入新算出的md5码，保存。
最后保存iso文件即可（我是用`UltraISO`操作的）。
