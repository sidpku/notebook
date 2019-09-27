---
title: ArcGIS 相关知识
mathjax: false
date: 2019-07-11 19:03:44
update: 2019-07-11 19:03:44
tag:
- arcgis
- log
- research
categories:
- log
---

# ArcGIS 相关知识

## 安装工作

### 安装环境

* windows10
* Net. Framework 4.8(要求4.5+)

### 安装资源

* 安装包及教程：[麻辣GIS](<https://malagis.com/arcgis-desktop-10-6-full-installation-tutorial.html>)
* Net.Framework：[Microsoft](<https://dotnet.microsoft.com/download>)

## 文件类型

### .ovr file

Drawing overlay file created by The Overlay Maker, a program used for **annotating maps**; supports drawn objects such as text, icons, and other vector and raster graphics; stores references to drawn resources and the map, but not the resource files themselves.

## 投影和几何变换

Projection and Transformations

参考资料：:book:邢超, 李斌. ArcGIS 学习指南[M]. 北京: 科学出版社, 2010. 289-309.

### 大地坐标系统（GCS）

大地坐标系统(Geographic Coordinate System, GCS)，以数学公式模拟的椭球体去拟合地球表面，按照坐标中心相对于地球中心的位置，可以分为两类

* 地心大地基准：椭球球心和地心重合  :chestnut: WGS-84 全称 World Geodetic System
* 本地大地基准：椭球球心和地心分离，椭球表面精确拟合局地地面 :chestnut: 北京-1954 :chestnut: 西安-1980

### 投影坐标系统（PCS）

与大地坐标系统不同，投影坐标系统不是对球体的空间描述，而是将三维空间中的球体投影到平面上来，单位从“度”转化为“米”。中国目前通用的PCS是高斯-克吕格投影方法，`北京-1954`&`西安-1980`都是使用了这个方法。

* 投影过程

高斯-克吕格投影方法，通过椭圆柱体切地球表面的一个经圈后，解析法将椭球面上的经纬度投影到圆柱面上，然后将圆柱面展开获得平面投影。（并不是很懂，具体参看`P292`）该方法属于等角变形，角度没有发生变化，经线和纬线之间的角度仍是90度。

* 分带投影

为了防止过度变形，在我国，1:2.5-1:50万地形图采用6^o^分带投影，1:1万及更大比例尺的地图使用3^o^分带投影方法。在不同的分带中，坐标值是相对于本分带中的坐标系统得到的相对坐标，故常在表示位置时加上`分带号`。

* 假偏移	

为了防止投影后，在坐标系统中得到负的坐标值（有可能会增加计算的复杂度），有可能将纵轴西移500km，称为`假东偏`，将横轴南移，称为`假北偏`。

### Arcgis中的坐标系统和坐标值

#### 存储策略

Arcgis系统中，一套数据同时有坐标系统和坐标值，两者相互对应，但分开存储，相互独立、分离。

#### 动态投影

在ArcMap中，添加不同坐标系统的数据，程序会自动调整成相同的坐标系统，按照变化后的坐标进行投影。这种投影方式称为`动态投影`。

#### 空间参考

通常空间参考指的是`坐标系统`，但`geodatabase`格式的数据，还有`坐标域`的部分。

坐标域指的就是地理信息系统中要素可能的取值范围。

### 坐标系统转换

:x: ​通过`ArcCatalog`改变坐标系统的方法不可取！这种方法只会改变坐标系统，却不能改变对应的坐标值。

坐标转换需要的输入：

* 元数据
* 元坐标系统
* 新坐标系统
* 坐标系统转换关系

#### 矢量转换

`ArcToolbox`:arrow_right:`Data Management Tools`:arrow_right:`Projections and Transformations`:arrow_right:`Feature`:arrow_right:`Project`

#### 标量转换

`ArcToolbox`:arrow_right:`Data Management Tools`:arrow_right:`Projections and Transformations`:arrow_right:`Raster`:arrow_right:`Project Raster`

## 地图编辑

## 类型转换

### polygon:arrow_right:raster

常常会需要将某一区域的shp文件转成栅格，然后利用栅格数据进行分析。为了使得不同的栅格文件数据能够对应，栅格数据的范围(extent)和精确度要保持一致。

使用Arctoolbox中的工具。

`ArcToolbox`:arrow_right:`转换工具` :arrow_right:`转为栅格`:arrow_right:`面转栅格`得到如下界面。

![](E:\document\blogs\Hexo\source\_posts\note-for-argis-install\Snipaste_2019-07-31_17-26-49.png)

* 输入要素：要处理的数据（.shp）
* 值字段：输出栅格带有的数据
* 输出栅格数据集：保存输出数据的文件
* 像元分配类型：重采样的方法
* 优先级字段：
* 像元大小：边长，以度为单位

点击`环境`可以进行进一步的设置。

* 处理范围:star:：可以设定栅格数据集的**边界(extent)**。

> 如何得到tif栅格文件？
>
> ​		在左侧图层上`右键`，选择`数据`:arrow_right:`导出数据`，可以设定数据的`像元大小`（e.g.长宽不同）、`文件类型`、`空值赋值`等。​