---
title: Python基础（5）- csv
date: 2017-08-05 12:00:00
categories:
- Python-基础
tags:
- Python
---

这里简单介绍下Python中的csv模块，应该蛮常用的。
和csv有关，一定要回合打开文件这类操作有关，这里先看下这个open函数
官方文档：[https://docs.python.org/3/library/functions.html#open](https://docs.python.org/3/library/functions.html#open)

# 1.open
> open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)Open file and return a corresponding file object. If the file cannot be opened, an OSError is raised.

file，就是我们要打开的文件地址；
mode，就是打开的方式，默认是只读文本（'rt')

![](http://upload-images.jianshu.io/upload_images/76024-6b900cd2865a1c71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/76024-2dbd703eb2d9b406.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

newline，和换行符有关，在网上找了个资料：[https://www.zhihu.com/question/19751023](https://www.zhihu.com/question/19751023)

![](http://upload-images.jianshu.io/upload_images/76024-5729473610100cb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

换行符看起来有点儿乱，以后如果一段问题了再研究下。
下面，我们先来看看csv

# 2. csv.reader
> csv.reader(csvfile, dialect='excel', **fmtparams)Return a reader object which will iterate over lines in the given csvfile.

我们的csv文件是这样的
![](http://upload-images.jianshu.io/upload_images/76024-247a9d0b9b1d4f0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
import csv

with open(r'D:\document\python_demo\employee_data.csv') as csvfile:
    emp_reader = csv.reader(csvfile)
    for row in emp_reader:
        print(row)

##
runfile('D:/document/python_demo/demo_open.py', wdir='D:/document/python_demo')
['lufei', '20', 'leader', 'onepiece', '100']
['namei', '19', 'teacher', 'onepiece', '999']
```

就csv文件来说，会有几个特点，比如字段之间的分隔符，换行符等，我们使用上面的dialect来指定
如果我们，现在将分隔符，替换为^
![](http://upload-images.jianshu.io/upload_images/76024-d2d41da40334e1cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们再次执行，就无法正确分割数据了
``` python
runfile('D:/document/python_demo/demo_open.py', wdir='D:/document/python_demo')
['lufei^20^leader^onepiece^100']
['namei^19^teacher^onepiece^999']
```

我们修改下代码，加上delimiter就行了，详情参考官网：[https://docs.python.org/3/library/csv.html#csv-fmt-params](https://docs.python.org/3/library/csv.html#csv-fmt-params)

``` python
import csv

with open(r'D:\document\python_demo\employee_data.csv') as csvfile:
    emp_reader = csv.reader(csvfile,delimiter='^')
    for row in emp_reader:
        print(row)

##
runfile('D:/document/python_demo/demo_open.py', wdir='D:/document/python_demo')
['lufei', '20', 'leader', 'onepiece', '100']
['namei', '19', 'teacher', 'onepiece', '999']
```

这时候，如果我们的数据中，含有分隔符，我们需要再加上封闭符，一般都会使用双引号，这里使用参数quotechar指定，默认是双引号
``` python
csv data:
lufei^20^leader^$one^_^piece$^100
namei^19^teacher^onepiece^999

import csv

with open(r'D:\document\python_demo\employee_data.csv') as csvfile:
    emp_reader = csv.reader(csvfile,delimiter='^',quotechar='$')
    for row in emp_reader:
        print(row)


result:
runfile('D:/document/python_demo/demo_open.py', wdir='D:/document/python_demo')
['lufei', '20', 'leader', 'one^_^piece', '100']
['namei', '19', 'teacher', 'onepiece', '999']
```

# 3.csv.writer
> csv.writer(csvfile, dialect='excel', **fmtparams)Return a writer object responsible for converting the user’s data into delimited strings on the given file-like object. csvfile can be any object with a write() method.

这里的用法都差不多，我们简单举个小例子，用官网的例子
``` python
with open('eggs.csv', 'w', newline='') as csvfile:
    spamwriter = csv.writer(csvfile, delimiter=' ',
                            quotechar='|', quoting=csv.QUOTE_MINIMAL)
    spamwriter.writerow(['Spam'] * 5 + ['Baked Beans'])
    spamwriter.writerow(['Spam', 'Lovely |Spam', 'Wonderful Spam']) 

result：
|Spam| |Spam| |Spam| |Spam| |Spam| |Baked Beans|
|Spam| |Lovely ||Spam| |Wonderful Spam|
```

这里用到了另一个参数：quoting，这个参数是针对quotechar来说的，
quotechar在ETL工具中叫做封闭符，是为了防止字段内容中出现分割符，我们需要区分到底是分隔符，还是字段内容，所以需要根据quotechar去判断；
quoting则是控制在什么情况下使用封闭符，他有几个选项
``` python
csv.QUOTE_ALL #所有字段都添加封闭符
csv.QUOTE_NONNUMERIC #在非数值字段加封闭符
csv.QUOTE_NONE #所有字段都不加
csv.QUOTE_MINIMAL #只在出现分隔符的字段旁加封闭符，默认
```

好了，csv的就简单分享到这里了。