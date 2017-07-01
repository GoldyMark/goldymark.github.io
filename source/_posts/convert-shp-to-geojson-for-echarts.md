---
title: 'Convert shp to geojson, for ECharts'
date: 2014-10-17 23:54:52
updated: 2014-10-17 23:54:52
comments: true
tags:
  - echarts
  - gis
  - shp
  - geojson
categories:
  - program
---

最近在用`AngularJS`+`ECharts`做图表，要用到镇街一级的当地地图，无奈`ECharts`只能提供到市一级（demo里有一个能显示到县/区一级，[here](http://echarts.baidu.com/echarts2/doc/example/map20.html)），故要用到`ECharts`的扩展地图功能。

<!-- more -->

扩展方法官方doc说得很明白了，[here](http://echarts.baidu.com/doc/doc.html#附录地图扩展)；

PS:官方doc里只提供了`jQuery`的写法，我这里补充一个`AngularJS`的写法：

**jQuery（官网写法）：**

```javascript
$.getJSON('geoJson/USA_geo.json', callback);
```

**AngularJS：**

```javascript
$http.get('geoJson/USA_geo.json', function(data){
    callback(data);// 其实就是把获取到的geoJSON传入callback函数
});
```

但我手头上只有`shp`的地图格式，而`ECharts`要用到`geojson`格式。百度google一番后，大致有如下两种方法：

1. 用工具转换，例如GDAL的ogr2ogr，下载地址：[here](http://trac.osgeo.org/gdal/wiki/DownloadingGdalBinaries)；

2. 在线转换，地址如下：

+ [http://ogre.adc4gis.com](http://ogre.adc4gis.com/)（可能墙）
+ [http://converter.mygeodata.eu](http://converter.mygeodata.eu/)（可能墙）
+ [http://www.mapshaper.org](http://www.mapshaper.org/) (备注：这个不知道是我不懂得用还是本身有缺陷，转换后的geojson不能用，而且会丢失字段，列在这里仅供参考)
+ [http://vis.sccas.cn/demo/map](http://vis.sccas.cn/demo/map)（`ECharts`作者@Kener-林峰 提供的）

先说一下**背景**：

我的shp文件的地理坐标是**`GCS_WGS1984`**，投影坐标系是**`WGS_1984_UTM_Zone_49N（Transverse_Mercator）`**。

**ogr2ogr：**

完整命令如下（Mac版，其他版本估计差异不大）：

```shell
/Library/Frameworks/GDAL.framework/Programs/ogr2ogr -f "geoJSON" -t_srs "WGS84" -s_srs "+proj=utm +zone=49 +datum=WGS84 +units=m +no_defs" "area.json" "area.shp"
```

**重点是-s_srs的参数；**

**ogre.adc4gis.com：**

1. 按网站上的说明，把`.shp, .dbf, .shx, .prj(可选)`打包成zip；

2. 在Convert to GeoJSON框里选择刚才打包的zip包；

3. 在Target SRS里输入**“WGS84”**（关键步骤）；

4. 点击“CONVERT TO GEOJSON”按钮，然后就会生成相应的geoJSON；


**converter.mygeodata.eu**

1. 同上，将shp、dbf等打包成zip包并上传提交；

2. 上传成功后，理论上能在地图里预览你的shp文件对应的区域；

3. 在Target vector format选项框里，选择GeoJSON(.json)；

4. 在Output coordinate system选项框里，选择WGS 84（关键步骤）；

5. 点击Convert now!；


adc4gis功能比较基本，mygeodata功能比较丰富（支持Vecotr data的编辑），大家各取所需吧。


不知道是不是编码方式没选对，我在两个网站转换出来的geojson中的中文都是乱码，不过我的字段也不多，手动改也不是很麻烦（但后来用ogr2ogr转换出来的就没问题）。

其实这三种方法的关键一步都是要把输出的坐标系统设置为**`WGS 84`**，其他都比较好理解。

由于本人对GIS不是很熟悉，光是google以上东西这些就花了我半天的时间了，在这里mark一下，希望有需要的人能少走弯路，毕竟大部分都不需要研究GIS，而只是单纯的想要个geojson文件而已（新版本的arcGIS的arcToolbox好像有转换成geojson的工具）。
