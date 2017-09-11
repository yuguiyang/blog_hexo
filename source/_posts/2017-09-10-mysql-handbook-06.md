---
title: MySQL-常用函数
date: 2017-09-10 08:00:00
categories:
- MySQL
tags:
- SQL
- MySQL
---
MySQL
常用函数

这里介绍下，MySQL中常用的函数，函数有太多太多，不一定都需要记住，只需要有个印象，需要的时候去文档中找一下，记住一些常用的就好了

## 数学函数

### abs(x)
返回x的绝对值
``` sql
select abs(-10),abs(10)
```

![abs](http://upload-images.jianshu.io/upload_images/76024-5a4d7d49d66d09b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

### ceil(x)、ceiling(x)
向上取整,比该值大的第一个整数
``` sql
select CEIL(9.3),CEIL(9.5),CEIL(9.6),CEIL(-9.5),CEIL(-9.1)
```

![ceil](http://upload-images.jianshu.io/upload_images/76024-3106d7b4937de867.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### floor(x)
向下取整，比该值小的第一个整数

``` sql
select FLOOR(9.3),FLOOR(9.5),FLOOR(9.6),FLOOR(-9.5),FLOOR(-9.1)
```

![floor](http://upload-images.jianshu.io/upload_images/76024-52ccb9f3d6295929.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### round(x),round(x,y)
四舍五入，round(x)，最近的一个整数，round(x,y)，这个y可以指定精度

``` sql
select ROUND(9.3),ROUND(9.6),ROUND(9.378,2),ROUND(9.455,2)
```

![round](http://upload-images.jianshu.io/upload_images/76024-d46d1e49943ffec7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### rand(),rand(x)
rand() 返回0~1之间的随机数
rand(x) 返回0~1之间的随机数，如果x值相等，则返回值相等

``` sql
select RAND(),RAND(),RAND(10),RAND(10)
```

![rand](http://upload-images.jianshu.io/upload_images/76024-80978f1eb1fd9dfa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 字符串函数
### 获取字符串长度
 length(str)，返回str的长度,这里要注意下中文，占3个长度,这里的长度单位是bytes
``` sql
mysql> select length('abc'),length('中国'),length('hi中国');
+---------------+----------------+------------------+
| length('abc') | length('中国') | length('hi中国') |
+---------------+----------------+------------------+
|             3 |              6 |                8 |
+---------------+----------------+------------------+
1 row in set
```
 CHAR_LENGTH(str)，返回str的长度，中文和英文一样，占1个字符，这里长度单位是字符
``` sql
mysql> select char_length('abc'),char_length('中国'),char_length('hi中国');
+--------------------+---------------------+-----------------------+
| char_length('abc') | char_length('中国') | char_length('hi中国') |
+--------------------+---------------------+-----------------------+
|                  3 |                   2 |                     4 |
+--------------------+---------------------+-----------------------+
1 row in set
```
### 字符串拼接
CONCAT(str1,str2,...)，将str1，str2拼接在一起
``` sql

mysql> select concat('h','e','gogo','中国');
+-------------------------------+
| concat('h','e','gogo','中国') |
+-------------------------------+
| hegogo中国                    |
+-------------------------------+
1 row in set
```
这里要注意下NULL，如果其中有参数为NULL，则结果为NULL
``` sql
mysql> select concat('h','e','gogo',NULL,'中国');
+------------------------------------+
| concat('h','e','gogo',NULL,'中国') |
+------------------------------------+
| NULL                               |
+------------------------------------+
1 row in set
```

CONCAT_WS(separator,str1,str2,...)，使用指定的separator进行拼接
``` sql
mysql> select concat_ws('^','e','gogo',NULL,'中国');
+---------------------------------------+
| concat_ws('^','e','gogo',NULL,'中国') |
+---------------------------------------+
| e^gogo^中国                           |
+---------------------------------------+
1 row in set
```
这里是如果有NULL，对结果是没有影响的，会直接忽略NULL值

### 剔除空格或指定字符
剔除字符串左右的空格
ltrim(str),剔除左侧空格
rtrim(str),剔除右侧空格
TRIM([{BOTH | LEADING | TRAILING} [remstr] FROM] str), TRIM([remstr FROM] str)
trim可以使用参数来控制剔除空格或者是指定的remstr，默认是空格
``` sql
mysql> select concat(ltrim('   hi   '),'oo'),concat(rtrim('   hi   '),'oo'),concat(trim('   hi   '),'oo');
+--------------------------------+--------------------------------+-------------------------------+
| concat(ltrim('   hi   '),'oo') | concat(rtrim('   hi   '),'oo') | concat(trim('   hi   '),'oo') |
+--------------------------------+--------------------------------+-------------------------------+
| hi   oo                        |    hioo                        | hioo                          |
+--------------------------------+--------------------------------+-------------------------------+
1 row in set

mysql> select trim('a' from 'aabbbccaa'),trim(leading 'a' from 'aabbbccaa'),trim(trailing 'a' from 'aabbbccaa');
+----------------------------+------------------------------------+-------------------------------------+
| trim('a' from 'aabbbccaa') | trim(leading 'a' from 'aabbbccaa') | trim(trailing 'a' from 'aabbbccaa') |
+----------------------------+------------------------------------+-------------------------------------+
| bbbcc                      | bbbccaa                            | aabbbcc                             |
+----------------------------+------------------------------------+-------------------------------------+
1 row in set
```

### 字符串填充
LPAD(str,len,padstr)，左侧填充
RPAD(str,len,padstr)，右侧填充
len是指定str的长度，如果不够，则使用padstr填充，如果超了，则进行截取
``` sql
mysql> select lpad('hi',6,'@'),lpad('higogo',4,'@'),rpad('hi',6,'@'),rpad('higogo',4,'@');
+------------------+----------------------+------------------+----------------------+
| lpad('hi',6,'@') | lpad('higogo',4,'@') | rpad('hi',6,'@') | rpad('higogo',4,'@') |
+------------------+----------------------+------------------+----------------------+
| @@@@hi           | higo                 | hi@@@@           | higo                 |
+------------------+----------------------+------------------+----------------------+
1 row in set
```

### 字符串截取
LEFT(str,len)，从左侧开始截取len个字符
RIGHT(str,len)，从右侧截取len个字符
SUBSTR(str,pos), SUBSTR(str FROM pos), SUBSTR(str,pos,len), SUBSTR(str FROM pos FOR len)，从指定pos开始截取len个字符
``` sql
mysql> select left('hello',1),left('hello',3),right('hello',3);
+-----------------+-----------------+------------------+
| left('hello',1) | left('hello',3) | right('hello',3) |
+-----------------+-----------------+------------------+
| h               | hel             | llo              |
+-----------------+-----------------+------------------+
1 row in set

mysql> select substr('hello',1),substr('hello',2),substr('hello' from 2);
+-------------------+-------------------+------------------------+
| substr('hello',1) | substr('hello',2) | substr('hello' from 2) |
+-------------------+-------------------+------------------------+
| hello             | ello              | ello                   |
+-------------------+-------------------+------------------------+
1 row in set

mysql> select substr('hello',1,3),substr('hello',2,3),substr('hello' from 2 for 2);
+---------------------+---------------------+------------------------------+
| substr('hello',1,3) | substr('hello',2,3) | substr('hello' from 2 for 2) |
+---------------------+---------------------+------------------------------+
| hel                 | ell                 | el                           |
+---------------------+---------------------+------------------------------+
1 row in set
```


### 大小写转换
LOWER(str) LCASE(str)，将str转为小写
UPPER(str) UCASE(str)，将str转为大写
``` sql
mysql> select lower('AppLE'),lcase('AppLE'),upper('AppLE'),ucase('AppLE');
+----------------+----------------+----------------+----------------+
| lower('AppLE') | lcase('AppLE') | upper('AppLE') | ucase('AppLE') |
+----------------+----------------+----------------+----------------+
| apple          | apple          | APPLE          | APPLE          |
+----------------+----------------+----------------+----------------+
1 row in set
```



### 更多字符串函数
参考官网： [https://dev.mysql.com/doc/refman/5.7/en/string-functions.html](https://dev.mysql.com/doc/refman/5.7/en/string-functions.html)


## 日期和函数
### 获取当前日期、时间
``` sql
SELECT CURRENT_DATE,CURRENT_DATE(),CURRENT_TIME,CURRENT_TIME(),CURRENT_TIMESTAMP(),NOW()
```

![当前日期、时间](http://upload-images.jianshu.io/upload_images/76024-d02a125fbe334c51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 时间戳相关函数
UNIX_TIMESTAMP() 返回当前时间的时间戳，
UNIX_TIMESTAMP(x) 返回指定日期的时间戳
FROM_UNIXTIME(x) 将时间戳转为日期
FROM_UNIXTIME(x,y) 将时间戳转为指定格式的日期

``` sql
mysql> select UNIX_TIMESTAMP(),UNIX_TIMESTAMP('2017-09-10');
+------------------+------------------------------+
| UNIX_TIMESTAMP() | UNIX_TIMESTAMP('2017-09-10') |
+------------------+------------------------------+
|       1505096065 |                   1504972800 |
+------------------+------------------------------+
1 row in set

mysql> select FROM_UNIXTIME(1505096033),FROM_UNIXTIME(1504972800),FROM_UNIXTIME(1505096033,'%Y-%m-%d');
+---------------------------+---------------------------+--------------------------------------+
| FROM_UNIXTIME(1505096033) | FROM_UNIXTIME(1504972800) | FROM_UNIXTIME(1505096033,'%Y-%m-%d') |
+---------------------------+---------------------------+--------------------------------------+
| 2017-09-11 10:13:53       | 2017-09-10 00:00:00       | 2017-09-11                           |
+---------------------------+---------------------------+--------------------------------------+
1 row in set

mysql> 
```

### extract
EXTRACT(unit FROM date)
返回日期/时间的单独部分，比如年、月、日、小时、分钟等等
date 参数是合法的日期表达式。unit 参数可以是下列的值：

![extract](http://upload-images.jianshu.io/upload_images/76024-9db81fd5fd206620.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` sql
mysql> select extract(YEAR FROM NOW()),extract(MONTH from now()),extract(HOUR from now());
+--------------------------+---------------------------+--------------------------+
| extract(YEAR FROM NOW()) | extract(MONTH from now()) | extract(HOUR from now()) |
+--------------------------+---------------------------+--------------------------+
|                     2017 |                         9 |                       10 |
+--------------------------+---------------------------+--------------------------+
1 row in set

mysql> 
```
### datediff、timediff、timestampdiff
datediff(*expr1*,*expr2*),获取2个日期相差的天数
``` sql
mysql> select datediff('2017-09-11 10:00:00','2017-09-08 00:00:00');
+-------------------------------------------------------+
| datediff('2017-09-11 10:00:00','2017-09-08 00:00:00') |
+-------------------------------------------------------+
|                                                     3 |
+-------------------------------------------------------+
1 row in set

mysql> 
```
> TIMEDIFF(*expr1*,*expr2*)，返回expr1-expr2的时间差
``` sql
mysql> select timediff('2017-09-11 10:00:00','2017-09-08 00:00:00');
+-------------------------------------------------------+
| timediff('2017-09-11 10:00:00','2017-09-08 00:00:00') |
+-------------------------------------------------------+
| 82:00:00                                              |
+-------------------------------------------------------+
1 row in set

mysql> 
```
> TIMESTAMPDIFF(unit,datetime_expr1,datetime_expr2)
> 返回指定unit的datetime_expr2 − datetime_expr1时间差
> unit可以是MICROSECOND (microseconds), SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR.
``` sql
mysql> select timestampdiff(DAY,'2017-09-11 10:00:00','2017-09-08 00:00:00');
+----------------------------------------------------------------+
| timestampdiff(DAY,'2017-09-11 10:00:00','2017-09-08 00:00:00') |
+----------------------------------------------------------------+
|                                                             -3 |
+----------------------------------------------------------------+
1 row in set

mysql> select timestampdiff(HOUR,'2017-09-11 10:00:00','2017-09-08 00:00:00');
+-----------------------------------------------------------------+
| timestampdiff(HOUR,'2017-09-11 10:00:00','2017-09-08 00:00:00') |
+-----------------------------------------------------------------+
|                                                             -82 |
+-----------------------------------------------------------------+
1 row in set
```

### 时间加减函数
对日期进行加减操作，有很多方法可以使用，最简单的是直接使用interval
``` sql
mysql> select now(),now() + interval 3 DAY,now()+interval 1 Hour;
+---------------------+------------------------+-----------------------+
| now()               | now() + interval 3 DAY | now()+interval 1 Hour |
+---------------------+------------------------+-----------------------+
| 2017-09-11 10:57:18 | 2017-09-14 10:57:18    | 2017-09-11 11:57:18   |
+---------------------+------------------------+-----------------------+
1 row in set
```
当然也可使用提供的函数
> ADDDATE(date,INTERVAL expr unit), ADDDATE(expr,days)
> DATE_ADD(date,INTERVAL expr unit), DATE_SUB(date,INTERVAL expr unit)

![日期加减](http://upload-images.jianshu.io/upload_images/76024-7aa2e3a9f08f3d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
``` sql
mysql> select now(),date_add(now(),interval 1 Day),adddate(now(),interval 3 Hour);
+---------------------+--------------------------------+--------------------------------+
| now()               | date_add(now(),interval 1 Day) | adddate(now(),interval 3 Hour) |
+---------------------+--------------------------------+--------------------------------+
| 2017-09-11 10:59:09 | 2017-09-12 10:59:09            | 2017-09-11 13:59:09            |
+---------------------+--------------------------------+--------------------------------+
1 row in set
```

### 更多日期、时间函数
参考官方介绍: [https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html](https:
//dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html)


## 条件判断函数
### if
> if(expr,v1,v2) 
> 如果expr为真，则返回v1，为假，则返回v2
``` sql
mysql> select if(1>0,'ok','no'),if(1=0,'ok','no');
+-------------------+-------------------+
| if(1>0,'ok','no') | if(1=0,'ok','no') |
+-------------------+-------------------+
| ok                | no                |
+-------------------+-------------------+
1 row in set

mysql> 
```

### ifnull
> ifnull(v1,v2)
> 如果v1的值为null，则返回v2，如果v1不为null，则返回v1
``` sql
mysql> select ifnull(99,20),ifnull(NULL,99);
+---------------+-----------------+
| ifnull(99,20) | ifnull(NULL,99) |
+---------------+-----------------+
|            99 |              99 |
+---------------+-----------------+
1 row in set

mysql> 
```

### case when
> 这里可以根据多个条件来判断，在不同的情况下，返回不同的值

``` sql
select 
    s_id,s_name,s_gender,
    case 
        when s_gender=0 then '男' 
        when s_gender=1 then '女' 
    end gender,
    s_birthday,
    s_hobby,
    c_id
from 
    t_student
;
```

![case when](http://upload-images.jianshu.io/upload_images/76024-019524cc95e1a303.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)