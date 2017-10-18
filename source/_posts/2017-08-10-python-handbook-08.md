---
title: Python基础（8）- 关键字yield
date: 2017-08-10 10:00:00
categories:
- Python-基础
tags:
- Python
---


前几天遇到了这个yield，不知道是干嘛的，这里学习整理下，主要参考了：
[如何理解Python关键字yield ](https://ask.hellobi.com/blog/pythoneer/7476)
[Python高级特性](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014317793224211f408912d9c04f2eac4d2af0d5d3d7b2000)

上面介绍的都很好，这里就根据自己的理解，简单整理下。

#1. 什么是迭代
> 常见的list、tuple等集合，我们会通过遍历，比如for循环来获取每一个元素，这就是迭代。这些可以遍历的对象，也叫做可迭代对象

小例子
``` python
a = [1,2,3]
print(a)
for i in a:
    print(i)
    
b = 'abcd'
print(b)
for i in b:
    print(i)
    
c = {'name':'lufei','age':20}
print(c)
for k in c:
    print(k)
for v in c.values():
    print(v)
for k,v in c.items():
    print(k,v)

#out
[1, 2, 3]
1
2
3
abcd
a
b
c
d
{'name': 'lufei', 'age': 20}
name
age
lufei
20
name lufei
age 20
```

我们怎样判断一个对象是否可以去迭代呢？可以使用collections模块的Iterable
``` python
print(type(a),isinstance(a,Iterable))
print(type(b),isinstance(b,Iterable))
print(type(c),isinstance(c,Iterable))
print(type(123),isinstance(123,Iterable))

#out
<class 'list'> True
<class 'str'> True
<class 'dict'> True
<class 'int'> False
```

# 2. 列表生成式(List Comprehensions)
一个非常简单的方式来生成list，像这样：
``` python
range(10)
Out[56]: range(0, 10)

list(range(10))
Out[57]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

list(x for x in range(10))
Out[58]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

list(x*x for x in range(10))
Out[59]: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

[x*x for x in range(3)]
Out[60]: [0, 1, 4]

#上面的for后面还可以加上if判断
[x for x in range(10) if x>5]
Out[61]: [6, 7, 8, 9]

#for循环也可以嵌套
[x+y for x in 'abc' for y in 'xyz']
Out[62]: ['ax', 'ay', 'az', 'bx', 'by', 'bz', 'cx', 'cy', 'cz']
```

# 3.迭代器
  前面，我们说了for循环和可迭代对象，像这种可以使用for循环不断取出先一个元素的对象，就叫做迭代器（Iterator）。迭代器不单单可以使用for循环来遍历，
还可以使用next()函数不断获取下一个元素，当没有下一个元素时，会抛出StopIteration异常。
我们可以使用collections模块的Iterator来判断一个对象是否为迭代器

``` python
from collections import Iterator

isinstance([1,2,3],Iterator)
Out[2]: False

isinstance('abc',Iterator)
Out[3]: False

isinstance({'name':'lufei','age':20},Iterator)
Out[4]: False
```

创建一个迭代器有3中方式：
* 为对象创建 __iter__()和__next__()方法
* 内置的iter()可以将可迭代对象转换为迭代器
* 生成器

``` python
a = [1,2,3]

type(iter(a))
Out[6]: list_iterator

isinstance(iter(a),Iterator)
Out[7]: True
```

# 4. 生成器
上面，我们使用list()或者[]，很简单方便的生成了一个列表，只要我们将[]替换为()，就创建一个一个generator。生成器可以一边循环，一边计算生成下一个元素，而不是像list一样，一下生成所有的数据。
``` python
(x+y for x in 'abc' for y in 'xyz')
Out[63]: <generator object <genexpr> at 0x0000021AE0FC9D58>

a = (x+y for x in 'abc' for y in 'xyz')

a
Out[66]: <generator object <genexpr> at 0x0000021AE0FC9518>

isinstance(a,Iterator)
Out[67]: True
```

通常，我们使用yield语句可以返回一个生成器，很多例子，这里都是举一个斐波那契数列
yield类似return，只不过他返回的是生成器，调用了next()之后，
``` python
def fab(n):
    a,b,i = 0,1,0
    while i<n:
        print(b)
        a,b = b,a+b
        i+=1

fab(5)

#out
1
1
2
3
5

#使用yield改造
def fab(n):
    a,b,i = 0,1,0
    while i<n:
        yield b
        a,b = b,a+b
        i+=1

for i in fab(5):
    print(i)
```

我们调用fab的时候，执行到yield，会返回一个生成器，当调用next()后，程序会回到yield停止的地方继续往下执行
这样，就是每次只生成当前元素，而不是一下子生成所有的元素；
当然，for循环替我们调用了next()，并处理了StopIteration异常。
好了，梳理好上面这些概念，到yield这里，其实还好，平时在理解下，多用用，好，就到这。