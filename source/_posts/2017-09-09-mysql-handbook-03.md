---
title: MySQL-关联查询
date: 2017-09-09 10:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
关联查询


前面，我们介绍的都是单表查询（就是只从一张表中获取数据），而实际应用的时候，我们都会同时查询多张表，这里，我们就介绍下，多表关联查询的使用。

> SQL join 用于根据两个或多个表中的列之间的关系，从这些表中查询数据

![关联查询.png](http://upload-images.jianshu.io/upload_images/76024-d28b309853b85c19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 前置知识
> 主键（Primary Key）：可以唯一确定一条记录的字段，比如学生表中的学生ID，生活中我们的身份证号
外键（Foreign Key）：指向另一张表的主键，比如学生表中的班级ID，班级ID是班级表中的主键，但在学生表中是外键

主键和外键可以在建表的时候指定，他可以在数据库层面，控制你的数据的完整性、一致性。

<!-- more -->

测试数据参考：[http://yuguiyang.github.io/2017/09/09/mysql-handbook-01/](http://yuguiyang.github.io/2017/09/09/mysql-handbook-01/)

## inner join
inner join 可以简写为 join，结果集是两张表中 **都存在的记录**，是一个交集，详情参考上面的图片。
比如：在学生表中，有一个班级ID，我们想根据班级ID，在班级表中找到班级信息
``` sql
select 
    *
from 
    t_student a 
-- 要关联查询的表
join 
    t_class b 
-- 使用什么字段去关联这两张表
on 
    a.c_id = b.c_id
;

```

![join](http://upload-images.jianshu.io/upload_images/76024-a5ae853962b43597.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## left join
左关联，以左边的表为主表，不管外键在右表中是否存在，左表的数据都会存在。
比如学生表中，有这样一条记录，他的班级ID是904，但是班级表中并没有904的班级信息，
所以，使用join的话是查不到这条记录的
``` sql
-- 2. left join 
-- 学生表为主表，包含所有学生信息
select 
    *
from 
    t_student a 
left join 
    t_class b 
on 
    a.c_id = b.c_id
;
```

![left join](http://upload-images.jianshu.io/upload_images/76024-fc4ed0f6e9def043.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## right join
右关联，和做关联类似，但已右表为主表
``` sql

-- 3. right join 
-- 班级表为主表，不管改班级是否有学生信息
select 
    *
from 
    t_student a 
right join 
    t_class b 
on 
    a.c_id = b.c_id
;
```
![right join](http://upload-images.jianshu.io/upload_images/76024-f5a4a9876a971a43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## full outer join
全关联，mysql没有full join 语法，我们可以通过使用union来实现
``` sql
select 
    *
from 
    t_student a 
left join 
    t_class b 
on 
    a.c_id = b.c_id
UNION
select 
    *
from 
    t_student a 
right join 
    t_class b 
on 
    a.c_id = b.c_id
;
```

![full join](http://upload-images.jianshu.io/upload_images/76024-f5d58640d50dbd22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## cross join
cross join 是对2个表做笛卡尔积
``` sql
select *from t_class a 
cross join t_class b 
order by a.c_id,b.c_id
```
![data-analyst-interview-sql-04-11](http://7xl61k.com1.z0.glb.clouddn.com/data-analyst-interview-sql-04-11.png-blog.photo)

## 小结
关联查询的话，我们主要是选择好主表，然后找好表与表之间的关联关系，注意多对多、一对多的这种关系，验证号结果数据就行了。