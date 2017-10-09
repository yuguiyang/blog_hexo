---
title: MySQL-子查询的使用
date: 2017-09-11 09:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
变量的使用

## 什么是子查询
子查询是将一个 SELECT 语句的查询结果作为中间结果，供另一个 SQL 语句调用。
像这样：
``` sql
-- 我们将学生表中的所有班级ID当做中间结果
select *from t_class where c_id in (select distinct c_id from t_student);
```

## 常用比较符
子查询最常用的用法：
> non_subquery_operand comparison_operator (subquery)
> 其中操作符通常为
> =  >  <  >=  <=  <>  !=  <=>

其他的都不说了，这里说下这个<=>,以前还真没用过
<=>和=比较类似，也是判断是否相等，相等返回1，不相等返回2
``` sql
mysql> select 1<=>1,1<=>2;
+-------+-------+
| 1<=>1 | 1<=>2 |
+-------+-------+
|     1 |     0 |
+-------+-------+
1 row in set

mysql> select 1=1,1=2;
+-----+-----+
| 1=1 | 1=2 |
+-----+-----+
|   1 |   0 |
+-----+-----+
1 row in set
```
和=不一样的地方，是对NULL的支持，用<=>可以判断是否为null，而等号则是出现null，结果就为null
``` sql
mysql> select 1<=>null,null<=>null,1=null,null=null;
+----------+-------------+--------+-----------+
| 1<=>null | null<=>null | 1=null | null=null |
+----------+-------------+--------+-----------+
|        0 |           1 | NULL   | NULL      |
+----------+-------------+--------+-----------+
1 row in set
```

<!-- more -->

### any、in、some
在子查询中，in平时用的比较多，这个any、some，这里简单说下any和some

> operand comparison_operator ANY (subquery)
> operand comparison_operator SOME (subquery)
> ``` comparison_operator 可以为 =  >  <  >=  <=  <>  != ```

any表示任意一个值，比如
\> any () :表示大于any中任意一个值，即>子查询中最小值
< any(): 表示小于any中任意一个值，即<子查询中最大值
= any(): 和in一样
<> any():比较好玩儿，如果子查询返回多个值，<> any会返回所有值

``` sql
select *from t_student 
where s_id > any(
    select s_id from t_student where s_id in (105,109,111)
);
```

![>any](http://upload-images.jianshu.io/upload_images/76024-84ca84a9cea73689.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` sql
select *from t_student 
where s_id = any(
    select s_id from t_student where s_id in (105,109,111)
);
```

![=any](http://upload-images.jianshu.io/upload_images/76024-79acd1b91a2fb372.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

some 和any是一样的就不多说了

### all
这个all我也没咋用过，all表示所有值，和any有点儿相反的意思
> operand comparison_operator ALL (subquery)

\> all ()：表示大于所有值，即>子查询中最大值
< all() : 表示小于所有值，即< 子查询中的最小值
= all(): 返回单个值时和=一样，返回多个值时貌似没啥用
<> all(): 和not in 一样

``` sql
select *from t_student 
where s_id > all(
    select s_id from t_student where s_id in (105,109)
);

```

![>all](http://upload-images.jianshu.io/upload_images/76024-8f0ce33bafb39e86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 标量子查询
这种情况下，子查询返回单个值，可以在任何地方使用它。
``` sql
select
    c_id,
    c_name,
    (select max(s_id) from t_student) as max_s_id
from 
    t_class;


select
    *
from 
    t_class 
where 
    c_id = (select max(c_id) from t_class);
```

## 行子查询
上面我们介绍的子查询，都是返回1列多行，行子查询的话，是返回1行多列
``` sql
-- 查询一班所有男生
select *from t_student
where (c_id,s_gender) = (901,0);
```

![行子查询](http://upload-images.jianshu.io/upload_images/76024-f925380b1f971526.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里也可以返回多行多列（也叫做表子查询）
``` sql
select *from t_student
where (c_id,s_gender) in (select 901,0 union select 902,0);
```

![多行多列](http://upload-images.jianshu.io/upload_images/76024-7c558cc3f28c665f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 参考资料
官方文档：
[https://dev.mysql.com/doc/refman/5.7/en/subqueries.html](https://dev.mysql.com/doc/refman/5.7/en/subqueries.html)