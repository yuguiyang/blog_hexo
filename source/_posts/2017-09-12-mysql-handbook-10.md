---
title: MySQL-自增列
date: 2017-09-12 08:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
自增列

# 什么是自增列
自增列就是一个自动增长的列，他没有什么业务含义，一般可能用来做主键，作为唯一标识。
自增列一般是一个整数，相比其他的UUID占用的存储更少，网络资源占用也少。如果考虑其他因素的话，UUID使用也很多。
实际应用还要考虑很多问题，不能单纯的使用


# 自增列是使用
我们可以再create table的时候，就定义好自增列
我们使用关键字 auto_increment 来指定。
``` sql
mysql> create table t_book_1(
    -> id int auto_increment,
    -> f_name varchar(10),
    -> primary key(id)
    -> );
```
这里的话，一定要让自增列是主键，不然会报错

<!-- more -->

``` sql
mysql> create table t_book_2(
    -> id int auto_increment,
    -> f_name varchar(10),
    -> f_other varchar(10)
    -> );
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
```
另一种方式就是先建表，后面再修改为auto_increment
``` sql
mysql> create table t_book_2(
    -> id int ,
    -> f_name varchar(10)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> alter table t_book_2 add primary key(id);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table t_book_2 modify id int auto_increment;
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

# 重置自增列
一般自增列，都是自动赋值的，我们先插入几条记录试试
``` sql
mysql> insert into t_book_2(f_name) values('aa');
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_book_2(f_name) values('bb');
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_book_2(f_name) values('cc');
Query OK, 1 row affected (0.01 sec)

mysql> select *from t_book_2;
+----+--------+
| id | f_name |
+----+--------+
|  1 | aa     |
|  2 | bb     |
|  3 | cc     |
+----+--------+
3 rows in set (0.00 sec)

```
这时候，自增列已经到3了，如果我们删除了一条数据，
``` sql
mysql> delete from t_book_2 where id=3;
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_book_2(f_name) values('dd');
Query OK, 1 row affected (0.00 sec)

mysql> select *from t_book_2;
+----+--------+
| id | f_name |
+----+--------+
|  1 | aa     |
|  2 | bb     |
|  4 | dd     |
+----+--------+
3 rows in set (0.00 sec)
```
序列并不会重新从3开始，如果我们清空，也是一样的。那怎样可以重置自增列呢？

## 使用truncate自动重置
``` sql
mysql> truncate table t_book_2;
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t_book_2(f_name) values('ee');
Query OK, 1 row affected (0.00 sec)

mysql> select *from t_book_2;
+----+--------+
| id | f_name |
+----+--------+
|  1 | ee     |
+----+--------+
1 row in set (0.00 sec)

```

## 手工修改
我们可以使用命令
``` sql
-- 我们可以修改为我们想要的值，但如果表中有数据的话，如果修改
-- 的值比当前最大值小，则会重置为最大值+1
alter table table_name auto_increment = 1;
```
``` sql
mysql> delete from t_book_2 where id=3;
Query OK, 1 row affected (0.00 sec)

mysql> alter table t_book_2 auto_increment=3;
Query OK, 0 rows affected (0.00 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> insert into t_book_2(f_name) values('hh');
Query OK, 1 row affected (0.00 sec)

mysql> select *from t_book_2;
+----+--------+
| id | f_name |
+----+--------+
|  1 | ee     |
|  2 | ff     |
|  3 | hh     |
+----+--------+
3 rows in set (0.00 sec)

```

## 直接drop，重新create
这个方法就不练习了，

# 手动给自增列赋值
在insert的时候，手动给自增列赋值，也是可以的，
手动赋值后，我们再插入的时候，就会使用当前自增序列的最大值
``` sql
mysql> insert into t_book_2(id,f_name) values(9,'xx');
Query OK, 1 row affected (0.00 sec)

mysql> insert into t_book_2(f_name) values('yy');
Query OK, 1 row affected (0.01 sec)

mysql> select *from t_book_2;
+----+--------+
| id | f_name |
+----+--------+
|  1 | ee     |
|  2 | ff     |
|  3 | hh     |
|  9 | xx     |
| 10 | yy     |
+----+--------+
5 rows in set (0.00 sec)

```

# 附录
发现一篇好文章，讲的还不错：[数据库自增列](http://www.trueeyu.com/2016/06/21/increment-column/)







