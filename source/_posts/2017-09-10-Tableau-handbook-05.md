---
title: 小白学习Tableau-使用计算进行聚焦
date: 2017-09-10 10:57:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---
Tableau实例
## 什么是聚焦
聚焦是一种技术，用于根据某个度量的值显示离散阈值。聚焦计算其实是一种可产生离散度量的特殊计算。

其实就是新构造了一个维度，这个维度将度量值进行分段，类似于年龄段这样的维度。
这个应用场景很多，比如我们有一个达标值，想要哪些数据达标，哪些未达标。

## 实例
### 数据源
使用Tableau中自带的“示例-超市”，其中的订单表

![数据源](http://upload-images.jianshu.io/upload_images/76024-ca3c0327cba4b659.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 交叉列表
我们来看每个地区，每个子类别的数量
![交叉列表](http://upload-images.jianshu.io/upload_images/76024-b45df3b52a76e9fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

现在，我们想要实现这样的功能，就是我有一个达标值500，只有数量达到500，才达标，怎样展示最好呢？

### 创建计算字段
我们来创建一个计算字段，如果数量大于500则达标，否则不达标

![创建计算字段](http://upload-images.jianshu.io/upload_images/76024-ce584722a9c7cd1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，把他拖到颜色上

![颜色](http://upload-images.jianshu.io/upload_images/76024-482bee397ffda796.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![颜色图例](http://upload-images.jianshu.io/upload_images/76024-2ae64b31b040e229.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到这里，我们就可以看到，已经按照Good、Bad来区分颜色，达标、不达标

### 使用参数
上面，我们已经达成了目的，思考下，这个达标值的问题，这个月是500，下个月可能会浮动为600或者400，但我们在代码中写死了500，不够灵活，我们就可以使用参数来控制。


![创建参数](http://upload-images.jianshu.io/upload_images/76024-d5545f23a0bb367f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![参数配置](http://upload-images.jianshu.io/upload_images/76024-6b6e05faf29e3361.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，我们修改上面的计算字段

![修改计算字段](http://upload-images.jianshu.io/upload_images/76024-424aca6d4258c774.png)

好了，最后，我们就可以动态的修改这个达标值了
![图片.png](http://upload-images.jianshu.io/upload_images/76024-4b80c6002deb5858.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 参考
官方文档：[示例 — 使用计算进行聚焦](https://onlinehelp.tableau.com/current/pro/desktop/zh-cn/calculations_calculatedfields_ex2spotlighting.html)



