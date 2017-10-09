---
title: numpy手册(5)-random模块
date: 2017-08-10 21:59:00
categories:
- "Python-Numpy"
tags:
- Python
- Numpy
---

Python
Numpy知识总结

***

numpy的random模块应该很常用，这里整理一下，
参考文章：
[http://www.mamicode.com/info-detail-507676.html](http://www.mamicode.com/info-detail-507676.html)
[https://docs.scipy.org/doc/numpy/reference/routines.random.html](https://docs.scipy.org/doc/numpy/reference/routines.random.html)

##  简单随机数据
``` python
numpy.random.rand(d0, d1, ..., dn)
Random values in a given shape.

Create an array of the given shape and populate it with random samples from a uniform distribution over [0, 1)
生成给定形状的随机值，随机值在[0,1)

import numpy as np

np.random.rand(10)
Out[2]: 
array([ 0.41103285,  0.19043225,  0.30385602,  0.19330136,  0.09727556,
        0.96518049,  0.29930132,  0.00633969,  0.64269577,  0.79953589])

np.random.rand(2,3)
Out[3]: 
array([[ 0.86213038,  0.56657202,  0.83083843],
       [ 0.48660386,  0.20508572,  0.4927877 ]])

np.random.rand(2,3,1)
Out[4]: 
array([[[ 0.06676746],
        [ 0.55548283],
        [ 0.04411342]],

       [[ 0.18659571],
        [ 0.02209355],
        [ 0.83529269]]])
```


<!-- more -->

``` python
numpy.random.randn(d0, d1, ..., dn)
返回指定形状的状态分布样本
For random samples from N(\mu, \sigma^2), use:

sigma * np.random.randn(...) + mu

np.random.randn(2,1)
Out[6]: 
array([[-0.29088142],
       [ 1.29634911]])

np.random.randn(2,2)
Out[7]: 
array([[ 0.24125164,  1.62201226],
       [ 0.10129715, -1.62001598]])


Two-by-four array of samples from N(3, 6.25):
2.5 * np.random.randn(2, 4) + 3
Out[8]: 
array([[ 5.09295036,  2.2706219 ,  3.26392307,  0.86550482],
       [ 7.59911261,  5.22543816,  2.0441248 ,  1.03322082]])
```

``` python
numpy.random.randint(low, high=None, size=None, dtype='l')
返回随机整数，左闭右开[low,high)

np.random.randint(low=1,high=10,size=8)
Out[10]: array([4, 2, 6, 7, 2, 4, 3, 8])

#high为空的话，直接[0,low)
np.random.randint(10,size=5)
Out[11]: array([1, 3, 7, 9, 9])

np.random.randint(low=1,high=10,size=(2,3))
Out[12]: 
array([[8, 3, 6],
       [4, 1, 9]])
```

``` python
numpy.random.random_integers(low, high=None, size=None)
返回随机整数，闭区间[low,high]

这个和random.randint类似，已经不推荐使用了

np.random.random_integers(low=1,high=5,size=5)
__main__:1: DeprecationWarning: This function is deprecated. Please call randint(1, 5 + 1) instead
Out[13]: array([3, 2, 2, 4, 5])
```

``` python
numpy.random.random_sample(size=None)
numpy.random.random(size=None)
numpy.random.ranf(size=None)
numpy.random.sample(size=None)
返回随机的浮点值，左闭右开区间[0.0, 1.0)

np.random.random_sample(8)
Out[14]: 
array([ 0.70353035,  0.79018004,  0.50390916,  0.46261548,  0.85556642,
        0.68129238,  0.07098945,  0.65927063])

np.random.random_sample([2,3])
Out[16]: 
array([[ 0.37546444,  0.50352846,  0.3496647 ],
       [ 0.02849239,  0.6035842 ,  0.32514876]])
```

## 排列
``` python
numpy.random.shuffle(x)
就地修改序列的顺序，类似于洗牌，打乱顺序

a = np.random.randint(low=1,high=10,size=10)

a
Out[18]: array([6, 2, 5, 5, 2, 2, 4, 9, 7, 8])

np.random.shuffle(a)

a
Out[20]: array([2, 8, 4, 7, 5, 2, 5, 6, 2, 9])
```

``` python
numpy.random.permutation(x)
返回一个随机排列
If x is an integer, randomly permute np.arange(x). If x is an array, make a copy and shuffle the elements randomly.

np.random.permutation(10)
Out[21]: array([8, 9, 7, 0, 6, 1, 2, 5, 3, 4])

np.random.permutation([1, 4, 9, 12, 15])
Out[22]: array([ 4, 12,  1,  9, 15])
```