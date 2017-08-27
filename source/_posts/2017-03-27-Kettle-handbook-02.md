---
title: Kettle手册（二）- 将数据导出为Excel
date: 2017-03-27 23:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
好了，我们先来看第一个例子，就是怎样将数据库中的数据，导出为Excel。
平时，如果我们需要将数据导出Excel的话，我们可能会直接复制，然后粘贴出来，但是数据量大的话，就不好用了；
或者使用Java等开发语言，写代码，导出Excel；或者一些数据库连接工具自带的导出功能。
其实，我们用Kettle的话，还是很方便的，但是平时用下来，Kettle的这个功能还是有些缺陷的，比如导出Excel2007+的时候，经常会报错，我一直也没有解决，这次记录博客顺便研究看看。

{% note primary %} 
# 1. Kettle的下载及使用
{% endnote %}

正式开始之前，我们简单说下Kettle的安装配置啥的，Kettle是绿色的，下载之后，直接运行就可以了
刚刚在网上下了个最新版的，后面，我们就是用这个7.0版本介绍官网地址：[Kettle官网](http://community.pentaho.com/projects/data-integration/)
	
![Kettle-handbook-02-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-01.png-blog.photo)

<!-- more -->

他这个网站，应该是不太好访问，有VPN的话，可以用起来，下载的话，大概800M左右，后面看看上传一份，昨天为了下载，现冲了个蓝灯的会员
解压以后，
![Kettle-handbook-02-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-02.png-blog.photo)
目录大概是这样的，我们会看到，这里有.bat文件和.sh文件，.bat就是我们在windows下使用的，.sh就是在Linux下使用的，我们找到 Spoon.bat这个文件，就可以启动Kettle了，奥，对了，得先安装下Java
![Kettle-handbook-02-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-03.png-blog.photo)
打开后，就是这样了，都是图形界面的，很好用
![Kettle-handbook-02-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-04.png-blog.photo)
Kettle中，主要有2中任务，一个是作业，一个是转换。一般来说，转换是一系列具体的操作，比如：调度SP，导出Excel等等；作业的话，就是按照一定流程来调度一系列转换。大概是这样，实际上，他们也是可以嵌套调用的，我们后面可以再讨论。

{% note primary %} 
# 2. 第一个转换-将数据导出为Excel
{% endnote %}

为了实现这个功能，我们需要：
1. 连接到数据库
2. 导出为Excel

首先，我们新建一个转换，
![Kettle-handbook-02-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-05.png-blog.photo)
新建，之后，我们可以看到，工具箱中，有很多的控件，我们都可以使用，
![Kettle-handbook-02-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-06.png-blog.photo)
很多我也没有用过，大家可以自行去尝试使用
好了，下面，我们就开始介绍我们这次的主题，导出数据到Excel
既然，是导出数据，说明我们肯定有一个源头，一个目标，源头是我们的一个数据库，我们得先连接到这个数据库
## 新建数据库连接
我们在主对象库中，DB连接上，右键单击，新建
![Kettle-handbook-02-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-07.png-blog.photo)
在这里呢，我们可以看到，有很多的数据库可以选择，我们只需要填写基本的连接信息就可以了
![Kettle-handbook-02-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-08.png-blog.photo)
我们这里连接的是Postgresql，配置好后，测试下，（坑，刚刚在windows上装的数据库，一直连不上，白名单都加好了，就是不行，结果是防火墙忘关了。。）
![Kettle-handbook-02-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-09.png-blog.photo)
好了，可以连接到数据库了，下面，我们得把数据导出啊，我们需要使用输入这个控件
输入下面，有很多的控件，我们这次只使用表输入，因为我们是直接从数据库中拿数据
![Kettle-handbook-02-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-10.png-blog.photo)
这里直接就是拖拽的，拖过去就行了，双击之后，可以编辑，这里我们就使用刚才的数据源连接，然后查询一张表，
![Kettle-handbook-02-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-11.png-blog.photo)
表的话，随便create一张就可以了，我们还可以预览数据
![Kettle-handbook-02-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-12.png-blog.photo)
源头好了，同样的思路，我们需要一个目标，就是输出了，输出到Excel
![Kettle-handbook-02-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-13.png-blog.photo)
同样的，我们托好之后，双击就可以编辑了，这里，我们主要关注2个配置，一个是excel保存地址，和字段
![Kettle-handbook-02-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-14.png-blog.photo)
我们选择一个地址，然后得，看下字段那个tab，
我们单击，获取字段，就可以从源头获取表中的字段了，当然，我们可以只导出，我们需要的字段，
![Kettle-handbook-02-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-15.png-blog.photo)
一步一步来的话，上面获取，可能会获取不到，因为，有一步，需要将2个控件，连起来，源头有了，目标也有了，得让他们关联起来啊，再Kettle中，这个连线叫做Hop（跳），就像一个管道一样，将数据流从一个点，指向另一个点。
![Kettle-handbook-02-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-16.png-blog.photo)
都好了，以后，我们就运行下
![Kettle-handbook-02-17.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-17.png-blog.photo)
和Java里面，一样，绿色的话，就代表成功了
![Kettle-handbook-02-19.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-19.png-blog.photo)
我们看下文件
![Kettle-handbook-02-18.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-02-18.png-blog.photo)
好了，我们的第一个例子，就成功了，还是很简单的，主要就是Kettle中控件的熟悉。









