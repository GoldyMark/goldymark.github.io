---
title: 解决Eclipse的快捷提示弹出过快的问题
comments: true
date: 2014-01-08 22:12:00
updated: 2014-01-08 22:12:00
tags:
  - tool
  - eclipse
categories:
  - program
  - tool
---

用过Eclipse的可能遇到这样一个问题，鼠标移到类、方法、变量等的时候会瞬间弹出快捷提示的窗口，而Eclipse里面貌似也没有设置弹出等待时间的选项，在这里我找到了解决方法，原文地址：http://untamedmind.wordpress.com/2008/11/08/changing-the-mousehovertime-add-delay-on-the-tooltips-pop-up/



大致操作就是：

1. 进入注册表，进入`[HKEY_CURRENT_USER]\Control Panel\Mouse`；
2. 在右侧窗口查找`MouseHoverTime`的字符串值；
3. 把其值修改为`300`（至于这个时间的单位是什么，我也不清楚，我个人建议300~400）；
4. 如果在第2步找不到这个字符串值，那就自己新建一个吧；
5. 重启即可。
