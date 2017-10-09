---
title: MySQL-变量的使用
date: 2017-09-10 09:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
变量的使用

这里，我们简单介绍下MySQL中变量的使用

## 变量的作用域
在MySQL中，变量的作用域主要有全局变量（Global）和会话级变量（Session）
全局变量会影响当前server上的所有操作，而会话级变量则只会影响当前用户的会话，server上其他用户的会话不受影响

## 系统变量
系统变量主要涉及MySQL Server的一些配置参数，系统变量，一般会在配置文件中进行配置，或者在使用命令启动MySQL的使用去指定。
当然，在MySQL启动后，也可以动态的去修改系统变量（有些变量是不能动态修改的，可以修改的变量请参照官方文档）。

<!-- more -->

那我们怎样动态的修改系统变量呢？
可以使用
> SET GLOBAL or SET SESSION
> @@global. | @@session.

``` sql
SET GLOBAL max_connections = 1000;
SET @@global.max_connections = 1000;

SET SESSION sql_mode = 'TRADITIONAL';
SET @@session.sql_mode = 'TRADITIONAL';
SET @@sql_mode = 'TRADITIONAL';
```

查看当前系统变量的值
``` sql
SHOW VARIABLES;

SHOW VARIABLES LIKE 'max_join_size';
SHOW SESSION VARIABLES LIKE 'max_join_size';

SHOW VARIABLES LIKE '%size%';
SHOW GLOBAL VARIABLES LIKE '%size%';
```

![查看系统变量](http://upload-images.jianshu.io/upload_images/76024-1bf110200938645e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 用户自定义变量
我们可以定义自己的变量，变量名为 @var_name，用户自定义变量都是会话级的
> SET @var_name = expr [, @var_name = expr] ...
> For [SET](https://dev.mysql.com/doc/refman/5.7/en/set-variable.html), either = or := can be used as the assignment operator.

我们也可以使用select来使用变量，或者给变量赋值，但必须使用:=
``` sql
mysql> SET @t1=1, @t2=2, @t3:=4;
Query OK, 0 rows affected

mysql> SELECT @t1, @t2, @t3, @t4 := @t1+@t2+@t3;
+-----+-----+-----+--------------------+
| @t1 | @t2 | @t3 | @t4 := @t1+@t2+@t3 |
+-----+-----+-----+--------------------+
|   1 |   2 |   4 |                  7 |
+-----+-----+-----+--------------------+
1 row in set
```
在使用自定义变量的时候，会有一个顺序的问题，
``` sql
set @p_rank:= 0;
select *,@p_rank,@p_rank:=@p_rank+1 from t_student;
```

![变量顺序](http://upload-images.jianshu.io/upload_images/76024-bb63e57e5a243008.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样虽然可以得到我们想要的结果，但是官方并不推荐

自定义变量，是在结果返回到客户端时，才进行处理的，所以我们HAVING, GROUP BY, or ORDER BY中使用的时候，并没有效果。


## 参考资料
官方文档：
[user-variables](https://dev.mysql.com/doc/refman/5.7/en/user-variables.html)
[server-system-variables](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)