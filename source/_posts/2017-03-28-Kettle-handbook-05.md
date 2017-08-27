---
title: Kettle手册（五）- 实例-增量同步数据
date: 2017-03-28 23:30:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
综合前面的几个例子，我们这里来是实现下增量数据的同步。
这里只是分享一种方法，实际工作中，还会有其他更好的方案。
增量同步的整体思路一般就是：首先拿到这张表的增量数据，怎么拿增量呢，源表需要有一个时间字段，代表该条记录的最新更新时间（及只要该条记录变化，该时间字段就会更新），当然有时间字段最好了，没有的话，可能需要做全表对比之类的操作；正常情况下，业务系统的表中都是有主键的，我们拿到增量数据之后，需要判断该记录的新插入的，还是更新的记录，如果是更新记录，我们需要先将数据加载到中间表，然后，根据主键将目标表中已存在的数据删除，最后再将本次的增量数据插入到目标表。

## 1.配置表的设计（元数据表）
首先我们需要一张配置表，来保存我们要增量同步的表的基本信息
``` sql
--元数据表
create table tm_etl_table(
	table_name varchar(50), --表名
	is_run int , --调度状态
	update_time timestamp,--表数据更新时间
	etl_insert_time timestamp --记录更新时间
);
```
我们初始化一条记录，我们就以这张ods_tm_book表
![Kettle-handbook-05-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-01.png-blog.photo)
一些基础表准备

<!-- more -->

``` sql
-- 源表
create table tm_book(id int,book_name varchar(10),latest_time timestamp);

-- 源表数据初始化
insert into tm_book(id,book_name,latest_time)
select x,x||'_name',clock_timestamp() from generate_series(1,10) x;

-- 目标表和中间表
create table ods_tm_book(id int,book_name varchar(10),latest_time timestamp,etl_insert_time timestamp);
create table staging_tm_book(id int,book_name varchar(10),latest_time timestamp);
```

源表中的数据
![Kettle-handbook-05-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-02.png-blog.photo)

## 2.同步数据的流程开发
整体流程是这样的，注意下，这个只是为了简单演示了这个增量的例子，实际应用的话得修改，这是有漏洞的。
![Kettle-handbook-05-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-03.png-blog.photo)

### 2.1更新元数据表的状态并获取表更新时间
就是我们第一个状态，我们更新tm_etl_table表，更新is_run=0，表示我们开始同步数据了，update_time，初始化为 ‘1970-01-01’，表示我们要拉取所有的数据
![Kettle-handbook-05-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-04.png-blog.photo)
![Kettle-handbook-05-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-05.png-blog.photo)
这里，我们将该表的更新时间作为变量，我们会在后面的转换中使用
![Kettle-handbook-05-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-06.png-blog.photo)

### 2.2 加载数据到中间表
我们这里，直接表对表，将数据插入到staging
![Kettle-handbook-05-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-07.png-blog.photo)
其中，表输入中，我们需要根据前面的更新时间变量，获取增量数据，注意，需要勾选上“替换SQL语句中的变量”
![Kettle-handbook-05-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-08.png-blog.photo)
这里，我们直接就表输出到中间表，每次都需将清空表数据
![Kettle-handbook-05-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-09.png-blog.photo)

### 2.3 加载数据到目标表
![Kettle-handbook-05-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-10.png-blog.photo)
这里，主要有3段脚本（为了方便，就这样吧），根据主键ID，清空目标表数据，然后，将数据插入到目标表，最后，更新tm_etl_table表中的记录状态
![Kettle-handbook-05-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-05-11.png-blog.photo)
好了，用Kettle实现一个增量的逻辑大概就是这样了，

## 3.小结

这里整理几个问题

### 3.1 中间表
这里的话，使用了中间表，Kettle中是有一个控件的，应该叫那个“插入/更新”，可以根据主键将数据更新掉，这个控件之前使用时，发现很慢，就一直没用，后面的话，可能会写个例子，简单测试看看。使用中间表，缓存下数据，也是不错的方法。

### 3.2 增量流程
目前公司中，增量抽取，是这样的，首先各个业务系统的数据导出到文本文件，然后批量将文件加载到数据仓库中（这里使用循环加载的）。因为每天的数据量比较大，如果知己到表的话，会很慢，使用文件，一些数据库都有批量加载的命令，很快很方便，比如：PostgreSQL中的copy命令，Greenplum中的外部表，还有Mysql中的load data等等。



