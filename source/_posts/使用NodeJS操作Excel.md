---
title: 使用NodeJS操作Excel
date: 2016-04-05 23:17:00
updated: 2016-04-05 23:17:00
comments: true
categories:
  - program
  - nodejs
tags:
  - nodejs
---

本周做了比较多数据导入、统计的工作（多是一次性工作，并非常态化），所以考虑使用NodeJS操作Excel（拼SQL、提取/生成数据、etc），提高工作效率（其实是Mac的Excel不好用……）。

<!-- more -->

首先安装`NodeJS`，这里不叙述；
然后用`NPM`安装`node-xlsx`，如果选择全局安装的话，加上`-g`参数
```
npm i node-xlsx
```

**Example Code**

*readExcel.js*
```javascript
const fs = require('fs');  //文件操作模块，类似Java中的File类
const xlsx = require('node-xlsx');

// 读取excel操作
const sheets = xlsx.parse(要读取的Excel路径);  //这一步会将Excel表的数据读取并转换成json格式

/**
 * do what you want to do
 * ...
 */

// 写入excel操作
let file = xlsx.build([{name:"sheet1","data":一个二维数组},{name:"sheet2","data":一个二维数组}])
fs.writeFileSync(写入文件的文件名.xlsx, file , 'binary');
```
