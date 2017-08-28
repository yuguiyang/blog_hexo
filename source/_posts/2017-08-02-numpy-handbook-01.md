---
title: numpy手册(1)-ndarray
date: 2017-08-02 21:59:00
categories:
- "Python-Numpy"
tags:
- Python
- Numpy
---
前面我们算是简单入门了Pandas，numpy也是数据分析中常用的，这里我们也来简单学习下。

# 1.numpy基本介绍
<blockquote class="blockquote-center">
numpy是Python的一种开源数值计算扩展，这种工具可以用来存储和处理大型矩阵。
一个用Python实现的科学计算包。
from 百度百科
</blockquote>

numpy有2种基本对象，
``` python
ndarray（N-dimensional array object）和 ufunc（universal function object）
```

ndarray是存储单一数据类型的多维数组，ufunc是能够对数组进行处理的函数。

<!-- more -->

# 2.ndarray
我们先来看看这个数组
首先，我们得引入numpy
``` python
import numpy as np
```

## 2.1 创建
数组初始化的话有很多方式：Array creation routines
我们可以直接使用list来初始化，array有很多的属性，比如大小，维度，元素个数
``` python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])
c = np.array([[1,2,3],[4,5,6],[7,8,9]])

print(a,type(a),',shape:',a.shape,',ndim:',a.ndim,',size:',a.size)
print(b,type(b),',shape:',b.shape,',ndim:',b.ndim,',size:',b.size)
print(c,type(c),',shape:',c.shape,',ndim:',c.ndim,',size:',c.size)
```

![numpy-handbook-01-01](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-01.png-blog.photo)

这里呢，我们定义了一维数组和二维数组，比如c，是3行3列的2维数组，元素个数是9个
``` python
numpy.array(object, dtype=None, copy=True, order='K', subok=False, ndmin=0)
```

这里，我们再说下这个shape，这个属性可以修改
``` python
#原来是4行3列
c = np.array([[1,2,3],[4,5,6],[7,8,9],[0,0,7]])
print(c)
#我们改为3行4列
c.shape=(3,4)
print(c)
#改为2行6列
c.shape=(2,6)
print(c)
```

![numpy-handbook-01-02](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-02.png-blog.photo)

这里需要注意下，如果某个轴的元素为-1，将根据数组元素的个数，自动计算长度
``` python
c.shape=(1,-1)
print(c)
c.shape=(-1,1)
print(c)
```
![numpy-handbook-01-03](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-03.png-blog.photo)

这里的shape是改变原来的数组，另一个method，可以创建一个改变shape的新数组，而原数组保持不变
``` python
c = np.array([[1,2,3],[4,5,6],[7,8,9],[0,0,7]])
print('c:',c)
d = c.reshape(2,6)
print('c:',c)
print('d:',d)
```
![numpy-handbook-01-04](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-04.png-blog.photo)

这里要注意的是，c和d共享内存数据存储内存区域，c变了，d也会变
``` python
print(c[0])
#修改c[0]
c[0]=[-9,-8,-3]
print('c:',c)
print('d:',d)
```
![numpy-handbook-01-05](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-05.png-blog.photo)

我们可以通过dtype来获取元素的类型，我们可以在初始化的时候，指定dtype
``` python
c = np.array([1,2,3])
print(c.dtype) #int32

d = np.array([1.1,3.3])
print(d.dtype) #float64
```

下面，我们来看看常用的初始化方法

### arange
通过指定开始值，结束值和步长来创建一维数组，这里不包过终值
``` python
arange([start,] stop[, step,], dtype=None)

np.arange(3)
Out[51]: array([0, 1, 2])

np.arange(1,10,3)
Out[52]: array([1, 4, 7])
```

### linspace
通过指定开始值，终值和元素个数，来创建数组，这里包括终值
``` python
np.linspace(1,10,5)
Out[53]: array([  1.  ,   3.25,   5.5 ,   7.75,  10.  ])

np.linspace(1,2,3)
Out[54]: array([ 1. ,  1.5,  2. ])
```

### np.zeros,np.ones

这2个函数可以初始化指定长度或形状的全0或全1的数组
``` python 
np.ones(3)
Out[202]: array([ 1.,  1.,  1.])

np.ones([2,2])
Out[203]: 
array([[ 1.,  1.],
       [ 1.,  1.]])

np.zeros(5)
Out[204]: array([ 0.,  0.,  0.,  0.,  0.])

np.zeros([4,3])
Out[205]: 
array([[ 0.,  0.,  0.],
       [ 0.,  0.,  0.],
       [ 0.,  0.,  0.],
       [ 0.,  0.,  0.]])
```

### np.empty

可以创建一个没有任何具体值得数组
``` python
np.empty(2)
Out[211]: array([  7.74860419e-304,   7.74860419e-304])

np.empty(2,dtype=int)
Out[214]: array([        -1, 2147483647])

np.empty((3,3),dtype=np.float64)
Out[215]: 
array([[  4.94065646e-324,   9.88131292e-324,   1.48219694e-323],
       [  1.97626258e-323,   2.47032823e-323,   2.96439388e-323],
       [  3.45845952e-323,   3.95252517e-323,   4.44659081e-323]])

```
这要注意下，empty初始化的都是没有意思的值，不一定会初始化为0

![numpy-handbook-01-06](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-06.png-blog.photo)

## 2.2 存取元素
这里直接粘贴一个例子，原始教程在这：http://old.sebug.net/paper/books/scipydoc/numpy_intro.html
``` python
>>> a = np.arange(10)
>>> a[5]    # 用整数作为下标可以获取数组中的某个元素
5
>>> a[3:5]  # 用范围作为下标获取数组的一个切片，包括a[3]不包括a[5]
array([3, 4])
>>> a[:5]   # 省略开始下标，表示从a[0]开始
array([0, 1, 2, 3, 4])
>>> a[:-1]  # 下标可以使用负数，表示从数组后往前数
array([0, 1, 2, 3, 4, 5, 6, 7, 8])
>>> a[2:4] = 100,101    # 下标还可以用来修改元素的值
>>> a
array([  0,   1, 100, 101,   4,   5,   6,   7,   8,   9])
>>> a[1:-1:2]   # 范围中的第三个参数表示步长，2表示隔一个元素取一个元素
array([  1, 101,   5,   7])
>>> a[::-1] # 省略范围的开始下标和结束下标，步长为-1，整个数组头尾颠倒
array([  9,   8,   7,   6,   5,   4, 101, 100,   1,   0])
>>> a[5:1:-2] # 步长为负数时，开始下标必须大于结束下标
array([  5, 101])
```

就2维数组来说

![numpy-handbook-01-07](http://7xl61k.com1.z0.glb.clouddn.com/numpy-handbook-01-07.png-blog.photo)

这是基本的获取方式，还有些高级的方法

### 使用整数序列

这里简单来2个练习，原文例子很多，就是通过下标来筛选数据
``` python
a = np.arange(-5,5,1)

a
Out[68]: array([-5, -4, -3, -2, -1,  0,  1,  2,  3,  4])

a[[1,3,5]]
Out[69]: array([-4, -2,  0])

### 使用布尔数组

按照传入的布尔数组，只有为True的才返回，也叫布尔型索引
``` python
a=np.array([-3,1,5])

a
Out[72]: array([-3,  1,  5])

a[[False,True,False]]
Out[73]: array([1])

a[[True,False,True]]
Out[74]: array([-3,  5])
```

# 3.附录（参考资料）
文档：
[https://docs.scipy.org/doc/numpy-dev/reference/index.html#reference](https://docs.scipy.org/doc/numpy-dev/reference/index.html#reference)

[numpy快速处理数据](http://old.sebug.net/paper/books/scipydoc/numpy_intro.html)