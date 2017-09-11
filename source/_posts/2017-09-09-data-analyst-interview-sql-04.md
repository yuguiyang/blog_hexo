---
title: SQL笔试题-MySQL练习题
date: 2017-09-09 11:00:00
categories:
- 笔试题
tags:
- SQL
- 笔试题
- MySQL
---
SQL笔试题

> 下面的SQL基于MySQL

下面整理些MySQL学习过程中，基本的练习题，题目来源于网上及个人总结。

## 测试数据
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
insert into t_class values(905,'五班');

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
insert into t_student values(111,'令狐冲',0,'1997-03-04','喝酒',904);

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

<!-- more -->

## 查询所有课程分数都大于50分的学生信息
``` sql
select 
	*
from 
	t_student 
where 
	s_id in (
		-- 按学生ID分组，看每个学生的最低分数是否大于50分
		select 
			s_id
		from 
			t_score 
		group by 
			s_id 
		having 
			min(score)>=50
);
```

![data-analyst-interview-sql-04-03](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-03.png-blog.photo)

## 换另一种方式，实现上一题
``` sql
select 
	*
from 
	t_student 
where 
	s_id not in (
		-- 查询分数小于50分的学生ID
		select s_id from t_score where score < 50
) and s_id in (
		-- 加上这个判断，是因为有的学生没有考试成绩
		select s_id from t_score
);
```

![data-analyst-interview-sql-04-03](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-03.png-blog.photo)

## 查询最少有2门课程都>=60分的学生信息
``` sql
select 
	*
from 
	t_student 
where 
	s_id in 
(
	select 
		s_id
	from 
		t_score 
	where 
		score>=60 
	group by 
		s_id 
	having 
		count(1)>=2
);
```

![data-analyst-interview-sql-04-04](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-04.png-blog.photo)

## 查询每个学生的个人信息及班级信息及所有科目分数，按照班级ID升序排列，课程分数降序排列
``` sql
select 
	a.*,b.c_name,c.course_name,c.score
from 
	t_student a
join 
	t_class b 
on 
	b.c_id = a.c_id
join 
	t_score c 
on 
	c.s_id = a.s_id
order by
	a.c_id asc,
	c.score desc 
;
```

![data-analyst-interview-sql-04-01](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-01.png-blog.photo)

## 查询每个学生的学生ID、学生姓名、班级名称、总分、平均分，按照班级名称升序、总分降序排列

``` sql
select 
	a.s_id,
	a.s_name,
	b.c_name,
	sum(c.score) total_score,
	avg(c.score) avg_score
from 
	t_student a
join 
	t_class b 
on 
	b.c_id = a.c_id
join 
	t_score c 
on 
	c.s_id = a.s_id
group by 
	a.s_id,
	a.s_name,
	b.c_name
order by 
	b.c_name,
	total_score desc
;
```

![data-analyst-interview-sql-04-02](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-02.png-blog.photo)

## 上一题，加上平均分大于60分
``` sql
select 
	a.s_id,
	a.s_name,
	b.c_name,
	sum(c.score) total_score,
	avg(c.score) avg_score
from 
	t_student a
join 
	t_class b 
on 
	b.c_id = a.c_id
join 
	t_score c 
on 
	c.s_id = a.s_id
group by 
	a.s_id,
	a.s_name,
	b.c_name
having 
	avg_score >= 60
order by 
	b.c_name,
	total_score desc
;
```
![data-analyst-interview-sql-04-05](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-05.png-blog.photo)

## 查询数学成绩比语文成绩高的所有学生信息及数学、语文的成绩

``` sql
-- 使用子查询
select
	x.*,
	a.course_name course_math,
	a.score math_score,
	b.course_name course_language ,
	b.score language_score
from
	t_student x
join 
(
	-- 所有学生的数学成绩
	select
		*
	from 
		t_score
	WHERE
		course_name = '数学'
) a
on 
	x.s_id = a.s_id
join (
	-- 所有学生的语文成绩
	select
		*
	from 
		t_score
	WHERE
		course_name = '语文'
) b 
on 
	a.s_id = b.s_id
and 
	b.score <= a.score
;

-- 2. 使用join
select
	x.*,
	y.course_name course_math,
	y.score math_score,
	z.course_name course_language ,
	z.score language_score
from
	t_student x
join 
	t_score y
ON
	x.s_id = y.s_id
and 
	course_name = '数学'
join 
	t_score z
on 
	z.course_name = '语文'
and 
	z.s_id = y.s_id 
and 
	z.score <= y.score
;
```

![语文成绩比数学成绩好的](http://upload-images.jianshu.io/upload_images/76024-90576d5281d0c7eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 查询英语和语文成绩都大于60的学生信息
``` sql
select x.* from t_student x
join (
	select 
		a.s_id
	from 
		t_score a
	where 
		a.score > 60
	and 
		a.course_name in ('英语','语文')
	group by
		a.s_id 
	having
		count(1) = 2
) y 
on y.s_id=x.s_id
;
```

![data-analyst-interview-sql-04-06](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-06.png-blog.photo)


## 查询每个学生的学生ID、学生姓名及平均分,按照平局分降序排列
``` sql
select 
	b.s_id,
	b.s_name,
	avg(a.score) avg_score
from 
	t_score a
join 
	t_student b 
on 
	b.s_id = a.s_id
group by 
	b.s_id,
	b.s_name
order by 
	avg_score desc
```
![data-analyst-interview-sql-04-07](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-07.png-blog.photo)

## 上一题，加上，平局分大于60分
``` sql
select 
	b.s_id,
	b.s_name,
	avg(a.score) avg_score
from 
	t_score a
join 
	t_student b 
on 
	b.s_id = a.s_id
group by 
	b.s_id,
	b.s_name
having 
	avg_score > 60
order by 
	avg_score desc
```

![data-analyst-interview-sql-04-08](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-08.png-blog.photo)

## 上上一题，加上只查看平均分最高的学生信息
``` sql
select 
	b.s_id,
	b.s_name,
	avg(a.score) avg_score
from 
	t_score a
join 
	t_student b 
on 
	b.s_id = a.s_id
group by 
	b.s_id,
	b.s_name
order by 
	avg_score desc
limit 1
;
```
![data-analyst-interview-sql-04-09](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-09.png-blog.photo)

## 上上上一题，加上平局分第3高的学生信息
``` sql
select 
	b.s_id,
	b.s_name,
	avg(a.score) avg_score
from 
	t_score a
join 
	t_student b 
on 
	b.s_id = a.s_id
group by 
	b.s_id,
	b.s_name
order by 
	avg_score desc
limit 1 offset 2
;
```
![data-analyst-interview-sql-04-10](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-10.png-blog.photo)

## 换一种方式实现上一题
``` sql
select 
	x.s_id,
	x.s_name,
	x.avg_score,
	@cur_rank := @cur_rank+1 as score_rank
from (
	select 
		b.s_id,
		b.s_name,
		avg(a.score) avg_score
	from 
		t_score a
	join 
		t_student b 
	on 
		b.s_id = a.s_id
	group by 
		b.s_id,
		b.s_name
) x
cross join (
	select @cur_rank:=0
)y
order by 
	x.avg_score desc;
```

![data-analyst-interview-sql-04-12](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-12.png-blog.photo)


