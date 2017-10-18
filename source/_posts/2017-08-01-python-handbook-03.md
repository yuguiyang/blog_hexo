---
title: Python基础（3）- lambda表达式
date: 2017-08-01 10:00:00
categories:
- Python-基础
tags:
- Python
---


这里简单整理下，lambda表达式相关内容。

# 什么是lambda表达式
lambda表达式，是一个匿名函数，用起来方便快捷一些
``` python
#lambda 参数:操作(参数)

fun_add = lambda x: x+1

print(fun_add(1))
print(fun_add(10))
```

这里，一个简单的加1的函数，看起来也很直观
``` python
fun_add = lambda x,y: x+y

print(fun_add(3,4))
print(fun_add(8,9))
```

这是x+y的函数，的确简洁很多
看网上，提到lambda表达式的话，都会提到函数式编程，一些常用的函数，像map,reduce,filter,sorted，

# map函数
map是Python内置的一个函数，接收2个参数，一个函数，一个或多个可迭代参数
``` python
map(func, *iterables) --> map object

Make an iterator that computes the function using arguments from
each of the iterables.  Stops when the shortest iterable is exhausted.
```

``` python
d1 = [1,2,3,4,5]

def add(x):
    return x+10

t = map(add , d1)
print('原来的list:',d1)
print('执行add后的list:',list(t))
```

我们定义了一个函数，对传入的参数加10，一个list
![](http://upload-images.jianshu.io/upload_images/76024-8893ba955b9085cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

map把这个函数，作用在每一个list的元素上，
这里呢，我们就可以用lambda表达式写，方便又直观
``` python
d1 = [1,2,3,4,5]
t = map(lambda x: x+10 , d1)
print('原来的list:',d1)
print('执行add后的list:',list(t))
```

我们也可以传2个list，这里会计算2个list的和
``` python
s1 = [1,2,3]
s2 = [4,5,6]
t = map(lambda x,y: x+y,s1,s2)
print(list(t)) ##[5, 7, 9]
```

# reduce函数
reduce会将function作用于sequence，function接收2个参数
``` python
reduce(function, sequence[, initial]) -> value

Apply a function of two arguments cumulatively to the items of a sequence,
from left to right, so as to reduce the sequence to a single value.
For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates
((((1+2)+3)+4)+5).  If initial is present, it is placed before the items
of the sequence in the calculation, and serves as a default when the
sequence is empty.
```

``` python
t = reduce(lambda x,y: x+y, [1,2,3])
print(t)   #6，((1+2)+3)=6

t = reduce(lambda x,y: x*10+y,[1,2,3])
print(t) # ((1*10+2)*10+3)=123
```

# filter函数
看名字，就是一个过滤的功能，对每个item调用function，只返回为True的
``` python
|  filter(function or None, iterable) --> filter object
|  
|  Return an iterator yielding those items of iterable for which function(item)
|  is true. If function is None, return the items that are true.
```

``` python
t = filter(lambda x: x<0,range(-5,5))
print(list(t)) #[-5, -4, -3, -2, -1]
```