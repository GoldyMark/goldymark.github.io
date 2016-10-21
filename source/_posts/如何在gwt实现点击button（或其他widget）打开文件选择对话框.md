---
title: 如何在GWT实现点击Button（或其他Widget）打开文件选择对话框
comments: true
date: 2013-09-18 22:42:00
updated: 2013-09-18 22:42:00
tags:
  - java
  - gwt
categories:
  - program
  - java
---

最近用`gwt-bootstrap`实现UI开发，还不错，但`FileUpload`这个组件太占位置，所以我想用个`Button`取而代之。但`FileUpload`本身没有实现`click()`方法，google一番后，其实还是要调用底层的实现（很奇怪`GWT`为什么没有实现这个简单的`click()`方法）。

代码如下：
```java
fileUpload.getElement().<InputElement> cast().click();
```

将代码放入`Button`的`click`事件监听器内即可。
