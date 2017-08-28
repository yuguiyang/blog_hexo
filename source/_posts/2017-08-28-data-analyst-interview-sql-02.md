---
title: SQL笔试题-行转列
date: 2017-08-28 13:11:00
categories:
- "笔试题"
tags:
- SQL
- 笔试题
---
SQL笔试题

> 下面的SQL基于PostgreSQL

# 1. 行转列

## 背景
我们写SQL的时候，经常会遇到一些列转行、行转列的情况，有的时候是为了展现需要，有的时候是代码里就得这样转一下。总之嘞，得掌握这个技巧。下面就开始我们的练习。


## 测试数据
``` sql
CREATE TABLE interview.tm_score
(
  stu_name character varying(20), -- 学生名称
  course_name character varying(20), -- 课程名称
  score numeric(10,0) -- 分数
)
WITH (
  OIDS=FALSE
);

-- 初始化数据
insert into interview.tm_score values('路飞','数学',100);
insert into interview.tm_score values('路飞','语文',62);
insert into interview.tm_score values('路飞','英语',98);
insert into interview.tm_score values('索隆','数学',40);
insert into interview.tm_score values('索隆','语文',57);
insert into interview.tm_score values('索隆','英语',40);
insert into interview.tm_score values('娜美','数学',42);
insert into interview.tm_score values('娜美','语文',44);
insert into interview.tm_score values('娜美','英语',28);
```
![data-analyst-interview-sql-02-01](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-01.png-blog.photo)

<!-- more -->

我们先来思考第一个问题：
我们怎样可以将课程变成列呢，类似交叉表那样？
最容易想到的方法，就是使用case when了

### case when
``` sql
select 
	stu_name,
	max(case course_name when '数学' then score else 0 end) as "数学", 
	max(case course_name when '语文' then score else 0 end) as "语文", 
	max(case course_name when '英语' then score else 0 end) as "英语"
	
from 
	interview.tm_score
group by 
	stu_name
;
```
![data-analyst-interview-sql-02-02](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-02.png-blog.photo)

### crosstab
在新版本的PostgreSQL中有一个extension，可以方便的实现行转列
我们需要先安装这个扩展，我看了下，PostgreSQL8.3以后的版本都可以安装
``` sql
create extension tablefunc
```
> 官网地址：[https://www.postgresql.org/docs/9.5/static/tablefunc.html](https://www.postgresql.org/docs/9.5/static/tablefunc.html)

安装完后，会有这么几个函数

![data-analyst-interview-sql-02-03](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-03.png-blog.photo)

我们可以使用crosstab来实现上面的行转列
``` sql
select *from crosstab(
	'select stu_name,course_name,score from interview.tm_score'
) as ct(stu_name varchar(20) ,"数学" numeric(10,0),"语文" numeric(10,0), "英语" numeric(10,0))
```
结果也是一样的，我们传入一个SQL，SQL里面返回3列（这3列都是默认处理的，第一列是主字段，第2列是要拆成列的字段，第3列是要显示的值），最后因为返回值是record，所以我们要定义一下类型。

这里，顺便看看这个函数的用法

#### 要转成列的字段有Null
是这样一种情况：不是所有的同学都有3门课程的分数
我们删掉了几条记录来模拟
![data-analyst-interview-sql-02-04](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-04.png-blog.photo)

这时候使用crosstab，结果会有问题

![data-analyst-interview-sql-02-05](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-05.png-blog.photo)

数据会自动从第一列开始，导致错误的数据
``` sql
-- crosstab(text source_sql, text category_sql)

select *from crosstab(
	'select stu_name,course_name,score from interview.tm_score','select distinct course_name from interview.tm_score'
) as ct(stu_name varchar(20) ,"数学" numeric(10,0),"语文" numeric(10,0), "英语" numeric(10,0))
```
这时候，数据就对了

![data-analyst-interview-sql-02-06](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-06.png-blog.photo)

#### source_sql 多于3列
这种情况是，我们查询的结果不单单有上面说的3列，可能还有其他字段，
比如，我们可以在上面的测试数据上加一个班级列
我们同样也是使用上面的方式解决

``` sql
alter table interview.tm_score add column stu_class varchar(10);

update interview.tm_score set stu_class='一班' where stu_name in ('路飞','索隆');
update interview.tm_score set stu_class='二班' where stu_name not in ('路飞','索隆');

select *from crosstab(
	'select stu_name,stu_class,course_name,score from interview.tm_score','select distinct course_name from interview.tm_score'
) as ct(stu_name varchar(20) ,stu_class varchar(10),"数学" numeric(10,0),"语文" numeric(10,0), "英语" numeric(10,0))

```

> 这里我们六个思考题，有这样一组数据：

![data-analyst-interview-sql-02-07](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-07.png-blog.photo)

我们想查询出这样格式的数据

![data-analyst-interview-sql-02-08](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-02-08.png-blog.photo)

大家可以练习下这个SQL该怎么写

# 列转行
我们可以把行转成列，那要怎样把列转成行呢？

## union all
最简单的方法是直接union all,既然要放到一列里面，那字段类型肯定要一样，所以直接根据不同字段去union all 就好了

``` sql
select stu_name,"数学" from xxx
union all 
select stu_name,"语文" from xxx
union all
select stu_name,"英语" from xxx
```

# 小结
这里简单介绍了几种常见的处理方法，实际使用中，一定还有更好地方法、或一些特殊的问题，欢迎大家分享、反馈。































