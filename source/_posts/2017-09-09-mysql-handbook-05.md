---
title: MySQL-分组排序
date: 2017-09-09 12:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
分组排序

使用过其他数据库的同学，一定知道那些好用的开窗函数，像什么rank() over(),sum() over() 等等，实现一些功能的时候很好用，但是呢，MySQL中并没有这些函数，那怎样实现这些功能呢？我们可以使用MySQL中的变量来实现这个功能。

我们先来看第一个类型的问题，关于排序的问题

## 排名
就是按照指定的字段，给每一条记录分配一个行号（序号），作为他的排名
在postgresql中可以使用像rank() over()这样的函数
``` sql
select 
    course_name,
    s_id,
    score,
    rank() over(order by score desc) rank_score
from 
    t_score
```
![mysql-handbook-05-02](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-05-02.png-blog.photo)

<!-- more -->

但在MySQL中，就没这么方便了，我们需要使用变量，来模拟实现
``` sql
select 
    course_name,
    s_id,
    score,
    -- 根据排序规则，每条记录增加1
    @rn:=@rn+1 as rank_score
    -- 初始化变量@rn，从0开始
from t_score cross join (select @rn:=0) x
order by score desc
;
```
![mysql-handbook-05-03](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-05-03.png-blog.photo)

细心的同学，会发现，上面的排名会有些差别，以前2条记录为例，因为都是87，所以postgresql中排名都是1，而在MySQL中呢，依然是自增，一个1，一个2，那我们是否能让他也是2个1并列呢？

当然可以，我们改一下SQL
只要我们判断一下，当前记录的score和上一条记录的score是不是一样就可以了，那怎样才能获取上一条记录的score呢？就是增加一个变量来记录上一条记录的score
``` sql
select 
    course_name,
    s_id,
    score,
    @pre pre_score,
    -- 判断当前score是否和上一条记录的score相等，
    -- 如果相等则使用和上一条一样的排名
    if(@pre=score ,@rn:=@rn,@rn:=@rn+1) as rank_score,
    @pre:=score cur_score
    -- 使用pre来保存上一条记录的score
from t_score cross join (select @rn:=0,@pre:=null) x
order by score desc
;
```
![mysql-handbook-05-04](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-05-04.png-blog.photo)

上面的排序还是有点儿问题，比如有2个第1之后，依然从第2开始，那我们能不能跳过直接从3开始呢？

我们回想下，最开始他的排名其实就是3，只是我们上边把它变成了2，那我们再有一个和之前一样的变量就够了。

``` sql
select 
    course_name,
    s_id,
    score,
    @pre pre_score,
    @rn_1:=@rn_1+1 rank_score_1,
    if(@pre=score ,@rn:=@rn,@rn:=@rn_1) as rank_score,
    @pre:=score cur_score
    
from t_score cross join (select @rn:=0,@pre:=null,@rn_1:=0) x
order by score desc
```
我们使用@rn_1来正常记录排名，当当前记录和上一条记录的score不一样时，我们使用@rn_1的排名信息。

![mysql-handbook-05-05](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-05-05.png-blog.photo)


## 分组排序
就是给每个分组中的数据，分别进行排名
如果有类似rank() over()的函数
``` sql
-- 在postgresql中
select 
    course_name,
    s_id,
    score,
    rank() over(partition by course_name order by score desc) rank_score
from 
    t_score
```

![mysql-handbook-05-01](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-05-01.png-blog.photo)

但在MySQL中，就没这么方便了，我们需要使用变量，来模拟实现
``` sql

```


> 先到这，明天接着写