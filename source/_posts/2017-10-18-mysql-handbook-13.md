---
title: MySQL-每周练习(2017-10-20)
date: 2017-10-18 20:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
每周练习

本周我们来一道数据处理的练习题。

还记得，小甲在周末培训的爬虫嘛，不知道大家学会了没，拉勾网的数据大家会爬取了吗？这道题和拉勾网有关哦。

#数据背景
假设你已经学会爬取数据了，可以将数据爬取下来，数据可能是这个样子(demo库中的tm_lagou_data表)：
``` sql
CREATE TABLE `tm_lagou_data` (
  `city` varchar(20) DEFAULT NULL COMMENT '城市',
  `company_short_name` varchar(100) DEFAULT NULL COMMENT '公司简称',
  `company_full_name` varchar(200) DEFAULT NULL COMMENT '公司全称',
  `company_industry` varchar(100) DEFAULT NULL COMMENT '所属行业',
  `company_location` varchar(100) DEFAULT NULL COMMENT '工作地点',
  `position_advantage` varchar(100) DEFAULT NULL COMMENT '岗位特点',
  `position_salary` varchar(20) DEFAULT NULL COMMENT '薪资',
  `position_workyear` varchar(20) DEFAULT NULL COMMENT '工作经验',
  `position_name` varchar(50) DEFAULT NULL COMMENT '职位名称',
  `position_first_type` varchar(100) DEFAULT NULL COMMENT '岗位类型-大类',
  `position_second_type` varchar(100) DEFAULT NULL COMMENT '岗位类型-小类',
  `position_lables` varchar(100) DEFAULT NULL COMMENT '岗位标签',
  `position_id` varchar(20) DEFAULT NULL COMMENT '岗位ID',
  `create_time` datetime DEFAULT NULL COMMENT '发布时间',
  `job_desc` text comment '岗位描述'
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='拉勾网-数据分析数据';
```

在Python中，我们没有过多的处理
![](http://upload-images.jianshu.io/upload_images/76024-81e13eefdf889f63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->


这一次呢，我们只需要关注一个字段即可company_industry，这是公司所属行业
这个行业呢，一般会有多个，像上海的这个挖财网，就是互联网+金融，有2个标签，中间是逗号分隔符

![](http://upload-images.jianshu.io/upload_images/76024-ab333f17e91f01ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 问题描述
现在问题是这样的，我们想要分析一下，数据分析师的行业分布，哪个行业招的数据分析师最多，哪个行业招的最少呢？

![](http://upload-images.jianshu.io/upload_images/76024-608826b1e0bb4648.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们的目的就是做出上面的图，直接使用Tableau，我是没有想到咋做，可以的话，欢迎分享给我，所以我们需要使用SQL来处理下

这里要注意下，行业标签通常是1个或者2个，但我们应该用灵活的SQL来处理，而不能用写死为2个的方法，那样太不灵活，我们要假设可能会有10个或者20个标签。

从SQL的角度来描述这道题，就是，源表：tm_lagou_data，我们需要获取每个行业的岗位数量（按照position_id来去重），例如上面的挖财网，他有2个标签，所以他会计算2次，移动互联网算一次，金融也算一次。
SQL结果类似下面这样：

![](http://upload-images.jianshu.io/upload_images/76024-1c05ae5492737e96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好啦，同学们，开始思考吧。













