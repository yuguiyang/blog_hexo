---
title: Kettle手册（一）- 序及Kettle简介
date: 2017-03-27 21:59:00
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---

{% note primary %} 
# 1. 序 
{% endnote %}

好久没有写博客了，新的一年总得留下点儿什么。目前主要负责数据仓库这一块任务，平时用用Kettle、SSIS这类ETL工具，而且工具的使用整理起来会方便些。所以先从Kettle开始，一点点整理下最近BI开发中掌握的知识。
以前有做过BI报表Cognos开发还有些入门级的Java，都在CSDN博客上，感兴趣的同学可以去看看：[于贵洋的博客](http://blog.csdn.net/yuguiyang1990)
![Kettle-handbook-01Kettle-handbook-01-01](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-01-01.png-blog.photo)
好了，下面就根据自己的经验和理解，整理下Kettle的知识。

<!-- more -->

{% note primary %}
# 2. Kettle简介 
{% endnote %}

Kettle这东西是干嘛的呢？
Kettle是一个开源的ETL工具，所以基本的数据抽取、转换、加载，他都可以。
比如：我要把一个mysql数据库的数据同步到一个Postgres数据库，我们有哪些办法呢?
大概会有:
1. 将数据导出为文本文件，使用PG的copy命令直接加载
2. 数据量少的话，直接拼接成insert脚本，批量插入
3. 一些开源的小工具，提供2种数据库直接的同步
4. Kettle

等等方法
再比如：
我每天需要统计一些系统中的异常数据，导出为Excel，用邮件发送给指定的开发人员处理，该怎样做呢？
1. Java或者其他开发语言做定时任务
2. Kettle   

和其他的ETL工具相比，他有什么优势呢？
> Kettle是基于Java开发的，是开源免费的，大家可以直接在网上下载；
跨平台，Windows，Linux都可以使用；使用起来简单快捷。

> 既然开源，相比于其他收费产品，劣势也就很显然了，比如稳定性啊，BUG修复处理啊，而且基于Java，性能上会差些。
当然都是相对来说，一般数据量使用或者逻辑不复杂的话，使用起来是很适合的。

刚刚也在社区上，发现了Kettle的视频，kettle视频，大家可以看看，应该用的到。
Kettle的基本介绍就这些，后面会根据实际的例子，来介绍下Kettle的使用。