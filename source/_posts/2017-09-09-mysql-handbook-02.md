---
title: MySQL-中文排序
date: 2017-09-09 09:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
中文排序


测试数据参考：[http://yuguiyang.github.io/2017/09/09/mysql-handbook-01/](http://yuguiyang.github.io/2017/09/09/mysql-handbook-01/)

以前还真没有关注这个中文排序的问题，这里记录下。

一张学生表
``` sql
select *from t_student;
```

![学生表](http://upload-images.jianshu.io/upload_images/76024-26480895de68e815.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们根据s_name来排序
``` sql
select *from t_student order by s_name;
```

![根据s_name排序](http://upload-images.jianshu.io/upload_images/76024-0f5ecf1f3512536d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的中文排序，是不对的，应该是由于字符集的问题，一般情况下，数据库中的编码都是使用UTF-8的，所以，对于中文会有问题。

从网上找到2中解决办法

<!-- more -->

## create table的时候加上binary属性（经测试，不好用）
注意下s_name字段，我们添加了binary属性
``` sql
CREATE TABLE `t_student_test` (
  `s_id` int(11) DEFAULT NULL COMMENT '学生ID',
  `s_name` varchar(20) binary DEFAULT NULL COMMENT '学生姓名',
  `s_gender` int(11) DEFAULT NULL COMMENT '学生性别 0-男,1-女',
  `s_birthday` date DEFAULT NULL COMMENT '出生日期',
  `s_hobby` varchar(100) DEFAULT NULL COMMENT '爱好',
  `c_id` int(11) DEFAULT NULL COMMENT '班级ID'
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='学生表';
```
这里，我试验是失败的，中文排序依然不对

## 在order by 后面，使用 convert函数
``` sql
select *from t_student order by convert(s_name using gbk);
```

![中文排序](http://upload-images.jianshu.io/upload_images/76024-27d3391d7061ae9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用convert函数是可以的，没有问题