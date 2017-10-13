---
title: MySQL-疑问汇总
date: 2017-10-12 18:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
疑问汇总

这里吧同学们遇到的问题都汇总起来，方便大家一起查阅。

## update at 2017-10-12

### 1. Workbench、Navicat是干嘛的，和SQL有啥关系？
Workbench和Navicat都是数据库管理工具，让我们可以方便快捷的管理和使用数据库。类似的工具有很多，免费的、收费的好多好多，就好像共享单车，ofo、mobike、小蓝单车、小鸣单车......用哪个都可以，看我们的选择了。

SQL呢？是一种查询语言，虽然和写代码编程有关，但是不用怕，记住九九乘法表，后面就是活学活用了。我们为了和数据库成为好朋友，得多和他聊天吧，但我们和他不是一个星球的，那咋办？巧了，他听得懂SQL，那我们学下SQL，就可以愉快的玩耍了。这里呢，我们还得注意一下，目前市场上在使用的数据库有很多，

![dbs](http://upload-images.jianshu.io/upload_images/76024-2f8349a4793e8a7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

SQL是一套标准，其他每个数据库都会实现规定的基本功能，然后拓展些自己的语法，所以很不幸，换一个数据库，我们的SQL不一定完全跑的通。
但是，只要我们掌握了标准的SQL语法然后针对不同的数据库，关注下特殊语法就可以了（会骑ofo，换了Mobike就不会骑了吗？）

综上所述，我们是通过管理工具（Workbench、Navicat......）去使用和管理数据库（MySQL.....），在管理工具中，我们使用SQL来和数据库互动。


### 2. Workbench的使用
参考：[MySQL-Workbench使用](http://yuguiyang.github.io/2017/10/12/mysql-handbook-08/)

### 3. sqlzoo第九题
题目地址: [sqlzoo](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial/zh)
首先是关于all的疑问

关于all可以参考这里先简单了解下：[MySQL-子查询的使用](http://yuguiyang.github.io/2017/09/11/mysql-handbook-08/)

SQL这样写是没问题的
``` sql
SELECT 
    name,
    continent,
    population 
FROM 
    world x
    -- 返回符合条件的所有数据
WHERE 
    -- 使用all，判断该州的所有人口数是否都 <= 25000000
    25000000  >= ALL(
        -- 我们使用子查询，获取每一个州的所有人口数
        SELECT 
            population 
        FROM 
            world y 
        WHERE  x.continent =y.continent
    )
```
* 疑问1： 能不能把这个all放到后面像这样

![](http://upload-images.jianshu.io/upload_images/76024-a537d0341c36d46f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
答案是不行的，这个不符合all的使用语法，具体语法参考官方文档：[https://dev.mysql.com/doc/refman/5.7/en/all-subqueries.html](https://dev.mysql.com/doc/refman/5.7/en/all-subqueries.html)

all一定要放在一个比较运算符的后面才行，所以，替换是不符合规范的，会报错
![](http://upload-images.jianshu.io/upload_images/76024-5038df8497d11e52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 疑问2：为什么下面的写法不对
``` sql
SELECT
    name,
    continent,
    population 
FROM 
    world 
WHERE  
    25000000 >= ALL(
        SELECT 
            MAX(population) 
        FROM 
            world  
        GROUP BY 
            continent 
    )
```

我们分析下题目，他是说要查询出州下面每个城市的人口数都小于等于25000000的记录
上面我们就是使用all来实现去判断每个州下面的所有城市的人口数都<= 25000000，现在呢，不想去判断每个城市的人口数了，我只要拿出该州下面最大的人口数，然后判断这个最大值是不是<= 25000000就可以了。
思路是对的，但是SQL稍微有些问题
``` sql
-- 要么返回所有记录，要么没有返回记录
SELECT
    name,
    continent,
    population 
FROM 
    world 
WHERE  
    -- 因为主表world和子查询没有关联关系，所以这里的all就变成了“每个州的最大人口数是不是都 <= 25000000
    -- 也就是说，下面子查询的返回值，如果都是 <= 25000000，那就会返回world表的所有数据；如果有一个州的人口数 >25000000，那就没有记录返回
    25000000 >= ALL(
        -- 获取每个州，最大的人口数
        SELECT 
            MAX(population) 
        FROM 
            world  
        GROUP BY 
            continent 
    ) 
```
所以，我们要做的改动，就是让主表和子查询有关联关系，即判断每一个州的最大值

![](http://upload-images.jianshu.io/upload_images/76024-6a16efbffe7d9a3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不使用all也可以，
``` sql
-- 查询符合条件的州即可
select name,continent,population from world where continent in (
    -- 使用having直接过滤，判断每个州的最大人口是否<= 25000000
    select continent from world group by continent having max(population) <= 25000000
)
```
好了，这个问题，也写到这，大家可以看看还有没有别的写法。









