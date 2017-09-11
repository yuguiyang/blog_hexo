---
title: MySQL-regexp
date: 2017-09-09 12:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
正则表达式

在前面，我们了解了like的使用，它可以做一些简单的匹配
> 在like中，我们使用%来代替任意个或多个字符，_表示任意单个字符

但在实际的应用场景中，我们可能还会需要更强大的匹配方式，比如“正则表达式”，这就需要使用regexp。

常用的正则表达式，更多的内容，大家可以百度下
![mysql-handbook-04-01](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-04-01.png-blog.photo)

<!-- more -->

我们就以前面的t_student表为例

## 查询学生姓路或乔的学生信息
``` sql
-- like 
select *from t_student where s_name like '路%' or s_name like '乔%'

-- regexp
select *from t_student where s_name regexp '^(路|乔)'
```
![mysql-handbook-04-02](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-04-02.png-blog.photo)

## 查询名字是以美结尾的学生信息
``` sql
select *from t_student where s_name regexp '美$'
```
![mysql-handbook-04-03](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-04-03.png-blog.photo)

## 查询学生爱好中，有吃肉的学生信息
``` sql
select *from t_student where s_hobby regexp '吃肉'
```
![mysql-handbook-04-04](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-04-04.png-blog.photo)

## 查询学生姓乔或者名字中有美字的学生信息
``` sql
select *from t_student where s_name regexp '(^乔)|美'
```
![mysql-handbook-04-05](http://7xl61k.com1.z0.glb.clouddn.com/mysql-handbook-04-05.png-blog.photo)


## 小结
这里先整理这几个简单的例子，后续会再补充。

可以参考官方的文档练习：[https://dev.mysql.com/doc/refman/5.7/en/regexp.html](https://dev.mysql.com/doc/refman/5.7/en/regexp.html)