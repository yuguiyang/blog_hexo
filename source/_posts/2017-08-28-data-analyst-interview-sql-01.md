---
title: SQL笔试题-累计值（月累计、年累计）
date: 2017-08-28 11:11:00
categories:
- "笔试题"
tags:
- SQL
- 笔试题
---
SQL笔试题

> 下面的SQL基于PostgreSQL

# 1. 累计值（月累计、年累计）

## 测试数据
``` sql
-- 图书的销量表
CREATE TABLE interview.tm_book_sales
(
  calendar_date date, -- 日期
  book_name character varying(100), -- 图书名称
  sales numeric(10,0) -- 销量
)
WITH (
  OIDS=FALSE
);

-- 测试数据
insert into tm_book_sales values('2017-01-01','解忧杂货店',56);
insert into tm_book_sales values('2017-01-02','解忧杂货店',100);
insert into tm_book_sales values('2017-01-03','解忧杂货店',70);
insert into tm_book_sales values('2017-01-06','解忧杂货店',11);
insert into tm_book_sales values('2017-01-07','解忧杂货店',65);
insert into tm_book_sales values('2017-01-08','解忧杂货店',9);
insert into tm_book_sales values('2017-01-09','解忧杂货店',30);
insert into tm_book_sales values('2017-01-10','解忧杂货店',56);
insert into tm_book_sales values('2017-01-01','雪落香杉树',18);
insert into tm_book_sales values('2017-01-02','雪落香杉树',40);
insert into tm_book_sales values('2017-01-03','雪落香杉树',2);
insert into tm_book_sales values('2017-01-04','雪落香杉树',22);
insert into tm_book_sales values('2017-01-05','雪落香杉树',48);
insert into tm_book_sales values('2017-01-07','雪落香杉树',71);
insert into tm_book_sales values('2017-01-09','雪落香杉树',73);
insert into tm_book_sales values('2017-01-10','雪落香杉树',37);
insert into tm_book_sales values('2017-02-03','解忧杂货店',5);
insert into tm_book_sales values('2017-02-05','解忧杂货店',46);
insert into tm_book_sales values('2017-02-06','解忧杂货店',35);
insert into tm_book_sales values('2017-02-07','解忧杂货店',10);
insert into tm_book_sales values('2017-02-09','解忧杂货店',30);
insert into tm_book_sales values('2017-02-10','解忧杂货店',12);
insert into tm_book_sales values('2017-02-13','解忧杂货店',43);
insert into tm_book_sales values('2017-02-01','雪落香杉树',10);
insert into tm_book_sales values('2017-02-04','雪落香杉树',78);
insert into tm_book_sales values('2017-02-10','雪落香杉树',50);
insert into tm_book_sales values('2017-02-20','雪落香杉树',93);
```

现在呢，我们有了图书每天的销量数据，下面，我们思考1个问题：
我想要统计每本图书的当月累计销量，应该怎么做呢？

<!-- more -->

如果只是单纯的统计每本书每个月的销量，熟悉SQL的同学，一定可以很快想到
``` sql
select 
	to_char(calendar_date,'yyyy-mm') month_name,
	book_name,
	sum(sales) 
from 
	interview.tm_book_sales 
group by
	to_char(calendar_date,'yyyy-mm'),
	book_name
order by 
	book_name,
	to_char(calendar_date,'yyyy-mm')
;

```
![data-analyst-interview-sql-01-01](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-01-01.png-blog.photo)

下面，我们来想下，这个月累计怎么做？
月累计值，其实就是当天的销量再加上当天之前的销量

## 自关联
通过 interview.tm_book_sales 表，我们可以获取每一天的销量，那要怎样获取每天历史的销量呢？
最简单的方式就是自关联了。
其实就是自己和自己去关联，来获取历史的销量
``` sql
select 
	t_today.calendar_date,
	t_today.book_name,
	sum(t_his.sales) sales_mtd
from 
	interview.tm_book_sales t_today
left join 
	interview.tm_book_sales t_his
on
	-- 图书名称相等
	t_his.book_name = t_today.book_name 
and 
	-- 月份相等，只统计当月
	to_char(t_his.calendar_date,'yyyy-mm') = to_char(t_today.calendar_date,'yyyy-mm')
and
	-- 获取当天之前的历史日期
	t_his.calendar_date <= t_today.calendar_date
group by
	t_today.calendar_date,
	t_today.book_name
order by 
	t_today.book_name,
	t_today.calendar_date
;
```
![data-analyst-interview-sql-01-02](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-01-02.png-blog.photo)

好了，上面，我们通过自关联，获取了每本图书的月累计销量，不要太高兴，我们观察下，就会发现些问题。
我们看看日期，就会发现，有些日期是没有销量的，比如：《解忧杂货店》2017-01-04，2017-01-05 就没有销量，但实际上，如果是累计值得花，他是应该有数据的，因为1号、2号、3号都有数据，就算4号当天没有销量，月累计也应该要算上前3天的销量，所以我们的SQL并不严谨，还得修改。

## 补全没有销量的日期
我们需要想办法补全缺失的日期，如果，t_today里面含有每一天每本书的数据就好了，这就要我们手动构造一个了。
``` sql
select 
	t_day.calendar_date,
	t_book.book_name
from
    -- 日期维度表，就是存放每一天
	base.dm_calendar t_day
cross join 
    -- 所有的图书信息
	(select distinct book_name from interview.tm_book_sales) t_book
where 
	t_day.month_id=201701
;
```
![data-analyst-interview-sql-01-03](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-01-03.png-blog.photo)

日期维度表的话，其实是数仓中必备的地基础维度中的一个，她里面就是存放了每一天的数据，和其他一些我们会常用的字段，后面写一篇文章详细介绍下。
我们通过笛卡尔积，生成了一张包含每一天每本的图书的一个全维度表。
``` sql
with t_dim as (
	select 
		t_day.calendar_date,
		t_book.book_name
	from 
		base.dm_calendar t_day
	cross join 
		(select distinct book_name from interview.tm_book_sales) t_book
	where 
		t_day.month_id=201701
)
select 
	t_dim.calendar_date,
	t_dim.book_name,
	sum(t_his.sales) sales_mtd
from 
	t_dim
left join 	
	interview.tm_book_sales t_today
on 
	t_today.calendar_date = t_dim.calendar_date
and
	t_today.book_name = t_dim.book_name
left join 
	interview.tm_book_sales t_his
on
	-- 图书名称相等
	t_his.book_name = t_dim.book_name 
and 
	-- 月份相等，只统计当月
	to_char(t_his.calendar_date,'yyyy-mm') = to_char(t_dim.calendar_date,'yyyy-mm')
and
	-- 获取当天之前的历史日期
	t_his.calendar_date <= t_dim.calendar_date
group by
	t_dim.calendar_date,
	t_dim.book_name
order by 
	t_dim.book_name,
	t_dim.calendar_date
;
```
![data-analyst-interview-sql-01-04](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-01-04.png-blog.photo)

好啦，补全了日期信息后，我们的月累计算是完成了，手工。

## 总结
简单总结下，通过上面的例子，我们要掌握什么呢？
首先是对业务的理解，比如上面的月累计的统计方法；然后根据统计方法，使用SQL去实现，一步步完善；还有对日期维度表的一个综合使用。
年累计的实现也是一样的，同学们可以自行练习下，有问题可以反馈。