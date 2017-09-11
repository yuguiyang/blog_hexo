---
title: MySQL-基本语法介绍
date: 2017-09-09 08:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
基本语法介绍

## 1. 什么是SQL
SQL（Structured Query Language）结构化查询语言，通过SQL，我们就可以查询数据库中的数据，而数据再数据库中又是以表的形式保存的，所以SQL查询，主要就是对表进行查询。

SQL的语法就和学习英语的语法、汉语拼音一样，满足给定的套路，去使用就可以了。

当我们拿到了数据库的连接信息，连接到一个数据库上，我们就可以开始写SQL了。

## 2. Navicat的使用
MySQL的客户端有很多，通常使用的，可能有Navicat，还有MySQL自带的workbench。
Navicat是收费产品，但在网上可以找到XX版，workbench是免费的。

这里以Navicat为例，简单介绍下。
![菜单-新建连接](http://upload-images.jianshu.io/upload_images/76024-c7a0409bf6b9a087.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![新建连接](http://upload-images.jianshu.io/upload_images/76024-4e9a9e1bce14c574.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在这里，输入数据库地址、用户名、密码等等就行了。
 
这一个一个圆柱形的，就是一个数据库实例，下面那些电子表格图标的就是表，数据就存储在表中。

![数据库及表](http://upload-images.jianshu.io/upload_images/76024-cfabf6503b213d37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

默认是不会看到表结构信息的，我们勾选下面的配置之后，就可以看到了
![对象信息](http://upload-images.jianshu.io/upload_images/76024-14e110741cf8f15d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![表结构信息](http://upload-images.jianshu.io/upload_images/76024-49271a0110fe3798.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3. 基本语法

### 数据准备
``` sql
create table t_student(
    s_id int comment '学生ID',
    s_name varchar(20) comment '学生姓名',
    s_gender int comment '学生性别 0-男,1-女',
    s_birthday date comment '出生日期',
    s_hobby varchar(100) comment '爱好',
    c_id int comment '班级ID'
) comment '学生表';

create table t_class(
    c_id int comment '班级ID',
    c_name varchar(20) comment '班级名称'
) comment '班级表';


create table t_score(
    sc_id int comment '成绩ID',
    s_id int comment '学生ID',
    course_name varchar(20) comment '课程名称',
    score numeric(10,0) comment '成绩'
    
) comment '成绩表';


insert into t_class values(901,'一班');
insert into t_class values(902,'二班');
insert into t_class values(903,'三班');


insert into t_student values(101,'路飞',0,'1990-01-26','吃肉,睡觉',901);
insert into t_student values(102,'娜美',1,'1995-10-05','足球,篮球',901);
insert into t_student values(103,'乔巴',0,'1992-08-11','唱歌,吃肉',901);
insert into t_student values(104,'鸣人',0,'1991-03-29','拉面,忍术',901);
insert into t_student values(105,'卡卡西',1,'1989-05-10','看书,吃肉',902);
insert into t_student values(106,'乌索普',1,'1988-02-02','跳舞,篮球',902);
insert into t_student values(107,'乔峰',0,'1990-12-12','跑步,羽毛球',902);
insert into t_student values(108,'段誉',0,'1990-12-13','吃肉,加班',903);
insert into t_student values(109,'虚竹',1,'1991-01-22','看电影,旅行',903);
insert into t_student values(110,'杨过',0,'2000-03-04','旅行',903);

insert into t_score values(1,101,'数学',39);
insert into t_score values(2,102,'数学',20);
insert into t_score values(3,103,'数学',54);
insert into t_score values(4,104,'数学',38);
insert into t_score values(5,105,'数学',70);
insert into t_score values(6,106,'数学',15);
insert into t_score values(7,107,'数学',75);
insert into t_score values(8,108,'数学',84);
insert into t_score values(9,109,'数学',87);
insert into t_score values(10,110,'数学',67);
insert into t_score values(11,101,'语文',73);
insert into t_score values(12,102,'语文',71);
insert into t_score values(13,103,'语文',82);
insert into t_score values(14,104,'语文',83);
insert into t_score values(15,105,'语文',36);
insert into t_score values(16,106,'语文',87);
insert into t_score values(17,107,'语文',74);
insert into t_score values(18,108,'语文',19);
insert into t_score values(19,109,'语文',29);
insert into t_score values(20,110,'语文',26);
insert into t_score values(21,101,'英语',55);
insert into t_score values(22,102,'英语',24);
insert into t_score values(23,103,'英语',38);
insert into t_score values(24,104,'英语',82);
insert into t_score values(25,105,'英语',12);
insert into t_score values(26,106,'英语',15);
insert into t_score values(27,107,'英语',50);
insert into t_score values(28,108,'英语',68);
insert into t_score values(29,109,'英语',77);
insert into t_score values(30,110,'英语',19);
```
### select
下面，我们来看看，怎样查看一张表的数据；SQL的语法呢，就好比是一个公式，初学的话我们去套用就可以了。

> SELECT 列名称 FROM 表名称
或者
SELECT * FROM 表名称

使用Navicat执行查询
``` sql
-- 查看学生表数据，指定字段
select s_id,s_name from t_student;
```

![学生表数据](http://upload-images.jianshu.io/upload_images/76024-9d37e4bef2b2880e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` sql
-- 查看所有字段
select *from t_student;
```

![学生表数据](http://upload-images.jianshu.io/upload_images/76024-eae6d5cb5d674dc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 排序
排序是很常用的功能，我们想要对结果集进行指定的排序，就要使用order by

> order by 字段名
默认升序，可以使用desc降序排列

``` sql
-- 学生ID降序排列
select *from t_student order by s_id desc;
```

![降序排列](http://upload-images.jianshu.io/upload_images/76024-d84dba85f11f7c15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

多字段排序
``` sql
-- 班级ID升序排列，班级ID一样的按学生ID降序排列
select *from t_student order by c_id,s_id desc;
```

![多字段排序](http://upload-images.jianshu.io/upload_images/76024-367ec9920a3fa2fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### limit 
> 指定返回记录的数目

我们上面，都是查询一张表所有的数据，有的时候表的数据量很大，或者我们只想看看排名前3的数据，我们就可以使用limit
``` sql
-- 学生ID降序排列,取前3条记录
select *from t_student order by s_id desc limit 3;
```

![limit](http://upload-images.jianshu.io/upload_images/76024-2e411a4cfdc54cdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### where
前面，我们可以查看一张表的所有数据、做排序、然后只取前几行，实际使用时，一定会有这样的需求，比如我们只想看学生ID是105的记录，就需要使用where了，它可以对数据进行过滤。

> SELECT 列名称 FROM 表名称 WHERE 列 运算符 值


![常用的运算符.png](http://upload-images.jianshu.io/upload_images/76024-9cf31d13fc46726f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` sql
-- 查看学生ID是105的学生信息
select *from t_student where s_id = 105;

-- 查看学生ID不是105的其他学生信息
select *from t_student where s_id <> 105 ;

-- 查看学生ID在103和108之间的学生信息
select *from t_student where s_id between 103 and 108;
```

这里还有一个操作符很常用，就是 in 和 not in。
in 表示在多个值中存在，加上not则表示不存在
``` sql
-- 查看学生ID是103,105，,107的学生信息
select *from t_student where s_id in (103,105,107);

-- 查看学生ID不在102中的其他学生信息
select *from t_student where s_id not in (102);
```

### like 
匹配字符串，像‘xxxx’一样
> %： 表示任意个或多个字符
_：表示任意单个字符

``` sql
-- 查看喜欢吃肉的学生信息
select *from t_student where s_hobby like '吃肉%';
select *from t_student where s_hobby like '%吃肉%';
```

![like使用](http://upload-images.jianshu.io/upload_images/76024-fcf94f5dcf94cf4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### and 和 or
上面，我们都是一个单独的过滤条件，实际上，我们的会有各种各样的情况，需要同时满足多种过滤条件，这就用到了 and 和 or。

> AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。
> 如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。
> 如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

``` sql
-- 查看班级ID是901的所有男生信息
select *from t_student where c_id=901 and s_gender=0;

-- 查看班级ID是901或者s_id大于107的学生信息
select *from t_student where c_id=901 or s_id > 107;
```
