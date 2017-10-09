---
title: MySQL-聚合函数
date: 2017-09-11 10:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
聚合函数

聚合函数也是函数的一种，比较常用，这里我们就单独拿出来介绍下。
聚合函数一般配合group by来使用，经常是用来对数据集中的数值求和、平均值啊这里类的。

## 聚合函数的默认特性
* 忽略NULL值
* 如果没有匹配的记录，返回NULL
* 如果没有使用group by，则默认对所有字段进行group by

## 常用聚合函数
这里的测试数据依然使用前面的数据，可以参考前面的文章。

### count
统计结果集的数量,没有结果时，返回0
``` sql
-- 我们以学生表为例，来统计每个班级的学生人数

select 
    c_id,count(1),
    count(s_id),
    -- 这里学生ID是唯一的，所以是否使用distinct是一样的
    count(distinct s_id),
    -- 统计班级的个数
    count(distinct c_id)
from 
    t_student 
group by 
    c_id;
```

![count](http://upload-images.jianshu.io/upload_images/76024-75e0809e902bed73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

当我们只使用count，不使用group by的时候，相当于对所有字段进行group by
``` sql
select 
    count(1)
from 
    t_student
;
```

![count without group by ](http://upload-images.jianshu.io/upload_images/76024-0936e61df32f5745.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里，我们再来看下count对于null值得处理
``` sql
select 
    -- count(*),是包括null值的
    count(*),
    -- count(1),也包括null值
    count(1),
    -- 指定字段的时候，是不包括null值的
    count(id),
    -- 同样也不包括null值
    count(DISTINCT id)
from (
    select 1 as id
    union select NULL
    union select 2
)x
```

![count-null](http://upload-images.jianshu.io/upload_images/76024-2c755387122916a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### avg、sum
计算结果集的平局值和结果集的累加和
``` sql
-- 统计每个学生的平均分和总分
select 
    s_id,
    avg(score),
    sum(score) 
from 
    t_score
group by 
    s_id
;
```

![avg&sum](http://upload-images.jianshu.io/upload_images/76024-3e4a029bc3a97faf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们再来看看avg和sum对null值的处理
``` sql
select 
    -- count指定字段，是不包括null值的
    count(score),
    -- avg也是不包括null值的
    avg(score),
    -- 如果想要统计score的记录，需要使用ifnull进行判断
    avg(IFNULL(score,0)),
    -- 求和
    sum(score)
from (
    select 10 as score
    union select NULL
    union select 20
)x;
```

![avg-with-null](http://upload-images.jianshu.io/upload_images/76024-744761351497d133.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### min、max
统计结果集的最小值和最大值
``` sql

select 
    s_id,
    -- 最低分
    min(score),
    -- 最高分
    max(score)
from 
    t_score
group by 
    s_id
;

```

![min-max](http://upload-images.jianshu.io/upload_images/76024-bdcc845786b4a896.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果没有匹配的记录，则返回null；如果结果集中有null值，会忽略null
``` sql

select 
    max(score),
    min(score)
from (
    select 10 as score
    union select NULL
    union select 20
)x;
```

![min-max-null](http://upload-images.jianshu.io/upload_images/76024-ae216becd745e4ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### group_concat
group_concat会将函数聚合后的所有值以逗号分隔，以字符串展现
``` sql
GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])
```

``` sql
select 
    s_id,
    -- 将该学生所有的成绩以逗号分隔显示
    group_concat(score)
from 
    t_score
group by 
    s_id
;
```

### having
在聚合函数的使用过程中，通常还会使用having来对聚合后的数据进行过滤
``` sql
select 
    s_id,
    sum(score) sum_score
from 
    t_score
group by 
    s_id
-- 总分大于150分
having 
    sum_score > 150
```

![having](http://upload-images.jianshu.io/upload_images/76024-c1efca915bfa8487.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







