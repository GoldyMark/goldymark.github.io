---
title: egit的一个小错误
comments: true
date: 2013-04-10 22:28:00
updated: 2013-04-10 22:28:00
tags:
  - git
categories:
  - program
  - git
---

用EGit和坚果云搭配开发环境，配置egit时提示一个错误：
```
The current branch is not configured for pull. No value for key branch.master.merge found in configuration
```


Google一番后发现原来是egit的config文件缺少了一些参数，在stackoverflow找到解决方法，如下图

{% asset_img p1.jpg %}

http://stackoverflow.com/questions/8820668/the-current-branch-is-not-configured-for-pull-no-value-for-key-branch-master-mer

谨以此文记录本人学习git的一个小过程。
