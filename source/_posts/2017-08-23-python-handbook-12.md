---
title: Python基础（10）- 时间函数小记
date: 2017-08-23 10:00:00
categories:
- Python-基础
tags:
- Python
---

这里整理下时间类函数的使用方法，主要是整理下，防止老是忘记。
参考官方网站：
[https://docs.python.org/3/library/datetime.html?highlight=strftime#strftime-strptime-behavior](https://docs.python.org/3/library/datetime.html?highlight=strftime#strftime-strptime-behavior)

# 1. strftime
> datetime.strftime(format)Return a string representing the date and time, controlled by an explicit format string.返回将datetime根据指定format格式化后的字符串

``` python
from datetime import datetime

now = datetime.now()

now
Out[3]: datetime.datetime(2017, 8, 23, 10, 21, 41, 802905)

now.strftime('%Y-%m-%d %H:%M:%S')
Out[5]: '2017-08-23 10:21:41'

 now.strftime('%Y-%m-%d %H:%M:%S %f')
Out[6]: '2017-08-23 10:21:41 802905'
```

这里主要是了解format格式就可以了
常用格式：
``` python
参考官方网站：http://www.runoob.com/python/att-time-strftime.html
    %y 两位数的年份表示（00-99）
    %Y 四位数的年份表示（000-9999）
    %m 月份（01-12）
    %d 月内中的一天（0-31）
    %H 24小时制小时数（0-23）
    %I 12小时制小时数（01-12）
    %M 分钟数（00=59）
    %S 秒（00-59）
    %a 本地简化星期名称
    %A 本地完整星期名称
    %b 本地简化的月份名称
    %B 本地完整的月份名称
    %c 本地相应的日期表示和时间表示
    %j 年内的一天（001-366）
    %p 本地A.M.或P.M.的等价符
    %U 一年中的星期数（00-53）星期天为星期的开始
    %w 星期（0-6），星期天为星期的开始
    %W 一年中的星期数（00-53）星期一为星期的开始
    %x 本地相应的日期表示
    %X 本地相应的时间表示
    %Z 当前时区的名称
    %% %号本身
```

# 2. strptime
> datetime.strptime(date_string, format)
Return a datetime corresponding to date_string, parsed according to format
根据format，将字符串转换为datetime

``` python
datetime.strptime('2017-08-23 10:21:41 802905','%Y-%m-%d %H:%M:%S %f')
Out[8]: datetime.datetime(2017, 8, 23, 10, 21, 41, 802905)

#就是strftime的一个逆过程，一个是日期到字符串，一个是字符串到日期
```