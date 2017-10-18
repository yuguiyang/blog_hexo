---
title: MySQL-Workbench使用
date: 2017-10-12 16:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
Workbench使用

简单介绍下Workbench的使用
Workbench是MySQL官方提供的一个可视化管理工具，跨多个平台而且免费的，详情参考[官网](https://www.mysql.com/products/workbench/)。
我们从下载地址下载，安装就行了

## 安装
![workbench 安装](http://upload-images.jianshu.io/upload_images/76024-7b7ea58cc50de3b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以单独下载，也可以使用提供的一个管理工具统一下载管理，管理工具提供了整个MySQL所有相关组件的统一管理维护，也挺方便。

![管理工具](http://upload-images.jianshu.io/upload_images/76024-f1ad915acdeb4815.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

我们这里就是用独立安装包了，下载完之后安装，可能会失败，因为他有2个依赖要先安装，失败的话，会提示你缺少的依赖。

![依赖](http://upload-images.jianshu.io/upload_images/76024-e72ac9b47dc55954.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里有个小坑，这里说的是.NET Framework 4.5，但实际上需要的是4.5.2貌似不太一样，所以刚刚一直安装失败，注意下就行了[.NET Framework 4.5.2](https://www.microsoft.com/en-us/download/details.aspx?id=42642)

安装过程就是一路Next就行了，没啥特殊的。
安装完之后，我们在桌面或者菜单找到这个

![](http://upload-images.jianshu.io/upload_images/76024-df302f1ca148ad08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

主界面应该是这样：

![主界面](http://upload-images.jianshu.io/upload_images/76024-464160f15ea2fc02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 新建连接
我们点击加号，新增一个连接
![新建连接](http://upload-images.jianshu.io/upload_images/76024-b965fbf385983a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在弹出的界面上，输入数据库的连接信息，就可以了，密码下图所示的地方输入

![密码](http://upload-images.jianshu.io/upload_images/76024-084e3359386f57c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入完成后，我们测试下连接是否正常
![测试连接](http://upload-images.jianshu.io/upload_images/76024-e52ac70f136395ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

都正常后，我们单击OK就好了

返回主界面后，可以看到我们刚刚新建的连接，我们双击就可以打开这个连接，开始我们的SQL学习了
![成功的连接](http://upload-images.jianshu.io/upload_images/76024-b9e4445db4c84577.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 功能介绍
![主界面介绍](http://upload-images.jianshu.io/upload_images/76024-95ed8b3f264f8166.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们目前主要关心的是SQL的学习，所以那些管理功能可以先忽略掉
我们主要关注“当前可以使用的数据库”和SQL的编辑界面就OK了

我们以demo库为例

![demo库](http://upload-images.jianshu.io/upload_images/76024-ed56a50a473520b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这里就是demo库中的表，数据库中的数据是以表的形式保存的
比如，我们看看学生表的表结构信息，单击这个表旁边的"i"图标

![表结构](http://upload-images.jianshu.io/upload_images/76024-4f73aaf40415718f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

右边会显示一个表的信息界面，字段啊，索引啊，触发器之类的，我们这里就看看DDL
![表信息](http://upload-images.jianshu.io/upload_images/76024-74048925f5ad4b09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里展示的就是t_student表的表结构信息
![ddl](http://upload-images.jianshu.io/upload_images/76024-a669737616fa7148.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## SQL练习
下面，我们看看，怎样去练习SQL
我们回到刚刚的query界面

![query界面](http://upload-images.jianshu.io/upload_images/76024-78f35b844bccc1ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，我们要选择我们使用的数据库
``` sql
use demo;
```

写好SQL，我们单击那个像闪电一样的图标执行，下面的输出窗口会显示执行结果，绿色的对号表示正确执行
![use db](http://upload-images.jianshu.io/upload_images/76024-8e7af97235422c15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，就可以各种SQL开始练习了
``` sql
select *from t_student;
```

![select](http://upload-images.jianshu.io/upload_images/76024-e17c39c9930c96af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

后面，我们就可以参考前面的博客，开始学习吧[MySQL-基本语法介绍](https://yuguiyang.github.io/2017/09/09/mysql-handbook-01/)







