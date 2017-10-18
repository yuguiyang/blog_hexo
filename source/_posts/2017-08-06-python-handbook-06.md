---
title: Python基础（6）- zip 
date: 2017-08-06 10:00:00
categories:
- Python-基础
tags:
- Python
---

这里记录一个函数的使用，zip
``` python
zip(iter1 [,iter2 [...]]) --> zip object

Return a zip object whose .__next__() method returns a tuple where
the i-th element comes from the i-th iterable argument.  The .__next__()
method continues until the shortest iterable in the argument sequence
is exhausted and then it raises StopIteration.
```

我们可以传入一个或多个可迭代对象，然后将对应位置的元素封装成一个tuple，然后把所有tuple封装为list返回
``` python
x = [1,2,3]

y = [4,5,6]

z = zip(x,y)

print(z)
<zip object at 0x000002D26251F208>

print(list(z))
[(1, 4), (2, 5), (3, 6)]

#在只有一个参数的时候
a = zip(x)

a
Out[8]: <zip at 0x2d261cab588>

list(a)
Out[9]: [(1,), (2,), (3,)]
```

如果2个参数的长度不一样，会以较短的为主
``` python
a = [1,2,3]

b = ['ONE', 'TWO', 'THREE', 'FOUR']

c = list(zip(a,b))

c
Out[31]: [(1, 'ONE'), (2, 'TWO'), (3, 'THREE')]
```

我们使用*，可以看做是unzip的过程
``` python
x = [1,2,3]

y = [4,5,6]

z = zip(*zip(x,y))

list(z)
Out[27]: [(1, 2, 3), (4, 5, 6)]
```

关于zip，有几个常用的场景，比如矩阵行列转换
``` python
a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

list(zip(*a))
Out[38]: [(1, 4, 7), (2, 5, 8), (3, 6, 9)]

list(map(list,zip(*a)))
Out[39]: [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

构造字典
``` python
a = [1,2,3]

b = ['one','two','three']

list(zip(a,b))
Out[42]: [(1, 'one'), (2, 'two'), (3, 'three')]

dict(list(zip(a,b)))
Out[43]: {1: 'one', 2: 'two', 3: 'three'}
```

参考博客：
[http://blog.csdn.net/shomy_liu/article/details/46968651](http://blog.csdn.net/shomy_liu/article/details/46968651)
[http://www.jb51.net/article/53051.htm](http://www.jb51.net/article/53051.htm)