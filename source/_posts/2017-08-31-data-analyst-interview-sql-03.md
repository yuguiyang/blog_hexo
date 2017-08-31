---
title: SQL笔试题-连续登录天数
date: 2017-08-31 13:11:00
categories:
- "笔试题"
tags:
- SQL
- 笔试题
---
SQL笔试题

> 下面的SQL基于PostgreSQL

# 1.用户连续登录天数

## 背景描述
现在我们有一张用户登录日志表，记录用户每天的登录时间，我们想要统计一下，用户每次连续登录的开始日期和结束日期，以及连续登录天数。

用户ID | 登录日期
---|---
1001 | 2017-01-01
1001 | 2017-01-02
1001 | 2017-01-04
1001 | 2017-01-06
1002 | 2017-01-02
1002 | 2017-01-03

同学们先思考下，整理下思路，如果没有思路或者某几个点不了解，就可以继续往下看了。

## 测试数据
``` sql
CREATE TABLE interview.tm_login_log
(
  user_id integer,
  login_date date
)
WITH (
  OIDS=FALSE
);

-- 这里的数据是最简化的情况，每个用户每天只有一条登录信息，
insert into interview.tm_login_log values(1001,'2017-01-01');
insert into interview.tm_login_log values(1001,'2017-01-02');
insert into interview.tm_login_log values(1001,'2017-01-04');
insert into interview.tm_login_log values(1001,'2017-01-05');
insert into interview.tm_login_log values(1001,'2017-01-06');
insert into interview.tm_login_log values(1001,'2017-01-07');
insert into interview.tm_login_log values(1001,'2017-01-08');
insert into interview.tm_login_log values(1001,'2017-01-09');
insert into interview.tm_login_log values(1001,'2017-01-10');
insert into interview.tm_login_log values(1001,'2017-01-12');
insert into interview.tm_login_log values(1001,'2017-01-13');
insert into interview.tm_login_log values(1001,'2017-01-15');
insert into interview.tm_login_log values(1001,'2017-01-16');
insert into interview.tm_login_log values(1002,'2017-01-01');
insert into interview.tm_login_log values(1002,'2017-01-02');
insert into interview.tm_login_log values(1002,'2017-01-03');
insert into interview.tm_login_log values(1002,'2017-01-04');
insert into interview.tm_login_log values(1002,'2017-01-05');
insert into interview.tm_login_log values(1002,'2017-01-06');
insert into interview.tm_login_log values(1002,'2017-01-07');
insert into interview.tm_login_log values(1002,'2017-01-08');
insert into interview.tm_login_log values(1002,'2017-01-09');
insert into interview.tm_login_log values(1002,'2017-01-10');
insert into interview.tm_login_log values(1002,'2017-01-11');
insert into interview.tm_login_log values(1002,'2017-01-12');
insert into interview.tm_login_log values(1002,'2017-01-13');
insert into interview.tm_login_log values(1002,'2017-01-16');
insert into interview.tm_login_log values(1002,'2017-01-17');

```

## 步骤拆解
我们首先要思考，怎样才算连续登录呢？就是1号登录，2号也登录了，这样就连续2天登录，那我们怎么知道2号他有没有登录呢？
一种思路是根据排序来判断：
我们来根据日期来排个名
``` sql
select 
	user_id,
	login_date,
	row_number() over(partition by user_id order by login_date) day_rank
from 
	interview.tm_login_log
;
```
![data-analyst-interview-sql-03-01](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-03-01.png-blog.photo)

现在，我们根据用户ID，对他的登录日期做了排序，但是我们还是没有办法知道，他是不是连续的。
我们根据这个排序再思考一下，对于一个用户来说，他的登录日期排序已经是连续的了，如果登录日期也是个数字，那我们根据每行的差值，就可以判断登录日期是否连续了。
我们换个角度，我们找一个起始日期，来计算一个相差的天数，用它去和排序相对比，就可以了。
``` sql
select 
	user_id,
	login_date,
	date_part('day',login_date::timestamp - timestamp'2017-01-01') day_interval, -- 间隔天数
	row_number() over(partition by user_id order by login_date) day_rank -- 日期排序
from 
	interview.tm_login_log
;
```
![data-analyst-interview-sql-03-02](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-03-02.png-blog.photo)

我们观察下数据，因为日期排序是连续的，我们统计的间隔天数都是一个起始日期，所以，如果登录日期是连续的，那么，排序-间隔天数的差值也应该是一样的

``` sql
select 
	user_id,
	login_date,
	date_part('day',login_date::timestamp - timestamp'2017-01-01') day_interval, -- 间隔天数
	row_number() over(partition by user_id order by login_date) day_rank, -- 日期排序
	(
		row_number() over(partition by user_id order by login_date)
	) - (
		date_part('day',login_date::timestamp - timestamp'2017-01-01')
	) diff_value
from 
	interview.tm_login_log
;

```
![data-analyst-interview-sql-03-03](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-03-03.png-blog.photo)

差值一样的记录，就是连续登录的日期

好了，连续登录的判断标准，我们已经确定了，下面就是把题目中要的数据查出来即可
``` sql

select 
	user_id,
	--diff_value, --差值
	min(login_date) start_date, --开始日期
	max(login_date) end_date, --结束日期
	count(1) running_days --连续登录天数
from (
	select 
		user_id,
		login_date,
		date_part('day',login_date::timestamp - timestamp'2017-01-01') day_interval, -- 间隔天数
		row_number() over(partition by user_id order by login_date) day_rank, -- 日期排序
		(
			row_number() over(partition by user_id order by login_date)
		) - (
			date_part('day',login_date::timestamp - timestamp'2017-01-01')
		) diff_value
	from 
		interview.tm_login_log
) base
group by user_id,diff_value
order by user_id,start_date
```
![data-analyst-interview-sql-03-05](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-03-05.png-blog.photo)

### 拓展：获取用户最大的连续登录天数及开始日期和结束日期
``` sql

with tmp as (
select 
	user_id,
	diff_value, --差值
	min(login_date) start_date, --开始日期
	max(login_date) end_date, --结束日期
	count(1) running_days --连续登录天数
from (
	select 
		user_id,
		login_date,
		date_part('day',login_date::timestamp - timestamp'2017-01-01') day_interval, -- 间隔天数
		row_number() over(partition by user_id order by login_date) day_rank, -- 日期排序
		(
			row_number() over(partition by user_id order by login_date)
		) - (
			date_part('day',login_date::timestamp - timestamp'2017-01-01')
		) diff_value
	from 
		interview.tm_login_log
) base
group by user_id,diff_value
) 
select 
	a.user_id,a.start_date,a.end_date,a.running_days
from 
	tmp a
join (
	select user_id,max(running_days) running_days from tmp group by user_id
) b on a.user_id = b.user_id
and a.running_days = b.running_days
;
```
![data-analyst-interview-sql-03-04](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-03-04.png-blog.photo)


## 小结
我们简单整理下思路，上面的例子，我认为主要是一个思路的介绍，核心就是我们要找到一个判断连续的方法，找到方法后，SQL自然就一步一步想出来了。
上面只是一种思路，一定还有更优的解法，欢迎大家反馈分享。

