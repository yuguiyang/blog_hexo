---
title: numpy手册(3)-Datetimes and Timedeltas
date: 2017-08-07 21:59:00
categories:
- "Python-Numpy"
tags:
- Python
- Numpy
---

Python
Numpy知识总结

***

这里说下numpy中日期、时间相关的使用。
主要参考：
[https://docs.scipy.org/doc/numpy-dev/reference/arrays.datetime.html](https://docs.scipy.org/doc/numpy-dev/reference/arrays.datetime.html)

## 基本使用
在numpy中，我们很方便的讲字符串转换成日期类型
``` python
import numpy as np

np.datetime64('2017-08-06')
Out[3]: numpy.datetime64('2017-08-06')

np.datetime64('2017-08')
Out[4]: numpy.datetime64('2017-08')

#我们可以通过参数，强制将数据格式化为我们想要的粒度
np.datetime64('2017-08' , 'D')
Out[5]: numpy.datetime64('2017-08-01')

np.datetime64('2017-08' , 'Y')
Out[6]: numpy.datetime64('2017')

a = np.array(['2017-07-01','2017-07-15','2017-08-01'] , dtype=np.datetime64)

a
Out[10]: array(['2017-07-01', '2017-07-15', '2017-08-01'], dtype='datetime64[D]')

#我们也可以使用arange函数初始化数组
b = np.arange('2017-08-01','2017-09-01',dtype=np.datetime64)

b
Out[12]: 
array(['2017-08-01', '2017-08-02', '2017-08-03', ..., '2017-08-29',
       '2017-08-30', '2017-08-31'], dtype='datetime64[D]')
```

<!-- more -->

## 日期计算
在numpy中，我们可以进行简单的日期计算

``` python
#2个日期相减，会得到相差的天数
np.datetime64('2017-08-03') - np.datetime64('2017-07-15')
Out[27]: numpy.timedelta64(19,'D')

#这里日期可以直接减去对应的天数
np.datetime64('2017-08-03') - np.timedelta64(20,'D')
Out[28]: numpy.datetime64('2017-07-14')

#这里粒度要一样，一个是D,不可以和M相减
np.datetime64('2017-08-03') - np.timedelta64(1,'M')
Traceback (most recent call last):

  File "<ipython-input-29-61beff16fb05>", line 1, in <module>
    np.datetime64('2017-08-03') - np.timedelta64(1,'M')

TypeError: Cannot get a common metadata divisor for NumPy datetime metadata [D] and [M] because they have incompatible nonlinear base time units


np.datetime64('2017-08') - np.timedelta64(1,'M')
Out[30]: numpy.datetime64('2017-07')
```

![日期格式](http://upload-images.jianshu.io/upload_images/76024-709e2454ac159e49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 工作日判断
numpy中提供了一个一些工作日判断的函数，比如，通常周一到周五是工作日
``` python
numpy.busday_offset(dates, offsets, roll='raise', weekmask='1111100', holidays=None, busdaycal=None, out=None)

First adjusts the date to fall on a valid day according to the roll rule, then applies offsets to the given dates counted in valid days.

#2017-08-01是周二
#2017-08-01的下一个工作日是2017-08-02
np.busday_offset('2017-08-01',1)
Out[32]: numpy.datetime64('2017-08-02')

#2017-08-01的下2个工作日是2017-08-03
np.busday_offset('2017-08-01',2)
Out[33]: numpy.datetime64('2017-08-03')
```

这时候，如果传入的日期是周末，就会报错了
``` python
#2017-08-05是周六
np.busday_offset('2017-08-05',2)
Traceback (most recent call last):

  File "<ipython-input-34-9f767204127b>", line 1, in <module>
    np.busday_offset('2017-08-05',2)

ValueError: Non-business day date in busday_offset
```

当然，我们可以通过参数来避免错误
``` python
roll : {‘raise’, ‘nat’, ‘forward’, ‘following’, ‘backward’, ‘preceding’, ‘modifiedfollowing’, ‘modifiedpreceding’}, optional



    How to treat dates that do not fall on a valid day. The default is ‘raise’.

            ‘raise’ means to raise an exception for an invalid day.
            ‘nat’ means to return a NaT (not-a-time) for an invalid day.
            ‘forward’ and ‘following’ mean to take the first valid day later in time.
            ‘backward’ and ‘preceding’ mean to take the first valid day earlier in time.
            ‘modifiedfollowing’ means to take the first valid day later in time unless it is across a Month boundary, in which case to take the first valid day earlier in time.
            ‘modifiedpreceding’ means to take the first valid day earlier in time unless it is across a Month boundary, in which case to take the first valid day later in time.

```

常用的可能是这个forward和backward
一个是向前取第一个有效的工作日，一个是向后取第一个有效的工作日

``` python
np.busday_offset('2017-08-05',2,roll='forward')
Out[35]: numpy.datetime64('2017-08-09')

np.busday_offset('2017-08-05',2,roll='backward')
Out[36]: numpy.datetime64('2017-08-08')

np.busday_offset('2017-08-05',0,roll='forward')
Out[37]: numpy.datetime64('2017-08-07')

np.busday_offset('2017-08-05',0,roll='backward')
Out[38]: numpy.datetime64('2017-08-04')

#判断是否为工作日
np.is_busday()

np.is_busday(np.datetime64('2017-08-05'))
Out[39]: False

np.is_busday(np.datetime64('2017-08-01'))
Out[40]: True


#判断时间段内，工作日天数
np.busday_count()

np.busday_count(np.datetime64('2017-08-01'),np.datetime64('2017-08-06'))
Out[41]: 4

np.busday_count(np.datetime64('2017-08-06'),np.datetime64('2017-08-01'))
Out[42]: -4

```