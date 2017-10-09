---
title: numpy手册(4)-ufunc
date: 2017-08-08 21:59:00
categories:
- "Python-Numpy"
tags:
- Python
- Numpy
---

Python
Numpy知识总结

***

这里，我们说下对数组操作的常用函数
## 常用函数
我们先说下接收一个参数的一元函数，比如 np.sqrt 开方函数
``` python
a = np.arange(10)

np.sqrt(a)
Out[45]: 
array([ 0.        ,  1.        ,  1.41421356,  1.73205081,  2.        ,
        2.23606798,  2.44948974,  2.64575131,  2.82842712,  3.        ])

a**0.5
Out[46]: 
array([ 0.        ,  1.        ,  1.41421356,  1.73205081,  2.        ,
        2.23606798,  2.44948974,  2.64575131,  2.82842712,  3.        ])
```

常用的一元函数
![常用一元函数](http://upload-images.jianshu.io/upload_images/76024-4e51ac598140bce5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![常用一元函数](http://upload-images.jianshu.io/upload_images/76024-fc8bedc9e7685c7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

还有些常用的二元函数，比如 add,subtract
``` python
a = np.array([1,2,3])

b = np.array([4,5,6])

a
Out[49]: array([1, 2, 3])

b
Out[50]: array([4, 5, 6])

np.add(a,b)
Out[51]: array([5, 7, 9])

np.subtract(a,b)
Out[52]: array([-3, -3, -3])
```
![常用二元函数](http://upload-images.jianshu.io/upload_images/76024-49d48098a4dc8dce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

## np.where
np.where 是三元表达式  x if condition  else y 的矢量化版本
``` python
numpy.where(condition[, x, y])

Return elements, either from x or y, depending on condition.

If only condition is given, return condition.nonzero().
```

我们根据condition的值，来确定是返回x的值，还是y的值
``` python
x = np.array([1.1,1.2,1.3,1.4,1.5])

y = np.array([2.1,2.2,2.3,2.4,2.5])

cond = np.array([True,False,True,True,False])

np.where(cond,x,y)
Out[56]: array([ 1.1,  2.2,  1.3,  1.4,  2.5])
```

np.where的第2个，第3个参数不一定是数组，也可以是标量值；
比如，有一个矩阵，我们想要将所有正值替换为2，负值替换为-2
``` python
a = np.random.randn(4,4)

a
Out[58]: 
array([[-0.62737481, -0.69252389,  0.34290602, -0.54297339],
       [ 0.57164788, -0.74841413,  1.02406934,  0.3089722 ],
       [-0.46170713,  2.17671732, -0.51607955, -0.44006653],
       [-0.13365017,  0.67350363, -0.51877754, -0.382468  ]])

np.where(a>0,2,-2)
Out[59]: 
array([[-2, -2,  2, -2],
       [ 2, -2,  2,  2],
       [-2,  2, -2, -2],
       [-2,  2, -2, -2]])

#负值的话，我们使用原来的值
np.where(a>0,2,a)
Out[60]: 
array([[-0.62737481, -0.69252389,  2.        , -0.54297339],
       [ 2.        , -0.74841413,  2.        ,  2.        ],
       [-0.46170713,  2.        , -0.51607955, -0.44006653],
       [-0.13365017,  2.        , -0.51877754, -0.382468  ]])
```

## 数组统计方法
我们可以统计数组或某个轴上的数据进行统计计算
``` python
a = np.array([1,2,3])

a.sum()
Out[68]: 6

a.max()
Out[69]: 3

a.min()
Out[70]: 1

a.mean()
Out[71]: 2.0

numpy.sum(a, axis=None, dtype=None, out=None, keepdims=<class numpy._globals._NoValue at 0x40ba726c>)
```

这类聚合函数，可以接收一个axis参数，指定要聚合的轴
``` python
a = np.arange(15).reshape(3,5)

a
Out[74]: 
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])

#在横轴上聚合
a.sum(axis=1)
Out[75]: array([10, 35, 60])

#在列上聚合
a.sum(axis=0)
Out[76]: array([15, 18, 21, 24, 27])
```

常用的统计函数
![常用统计函数](http://upload-images.jianshu.io/upload_images/76024-8dc12d28fc16bcaa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![常用统计函数](http://upload-images.jianshu.io/upload_images/76024-6df0f4b1c3e9098c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 用于布尔数组的方法
   对于布尔数组来说，执行上面的统计函数，会是将True转成1，False转成0
``` python
b = np.array([True,False,True,True])

b.sum()
Out[78]: 3

#b中有True
b.any()
Out[79]: True

#b中不都为True
b.all()
Out[80]: False
```

还有2个函数any和all，可以判断数组中是否存在一个或多个True

## 排序
我们可以对数组进行排序
``` python
ndarray.sort(axis=-1, kind='quicksort', order=None)
Sort an array, in-place.

a = np.random.randn(10)

a
Out[82]: 
array([ 0.02418202, -1.86975588,  0.00273745,  0.22470742,  1.10362729,
        0.75344308, -0.89005284, -0.94833805,  1.37111527,  1.22149417])

a.sort()

a
Out[84]: 
array([-1.86975588, -0.94833805, -0.89005284,  0.00273745,  0.02418202,
        0.22470742,  0.75344308,  1.10362729,  1.22149417,  1.37111527])
```

ndarray是就地排序，直接排序原数组；
np.sort则是返回一个排序后的数组

## 其他集合函数
比如unique，可以获取数组的唯一值
``` python
a = np.array([1,3,3,3,5,5,2,1])

a
Out[90]: array([1, 3, 3, 3, 5, 5, 2, 1])

np.unique(a)
Out[92]: array([1, 2, 3, 5])
```

![集合运算](http://upload-images.jianshu.io/upload_images/76024-82078f9adafe2ca8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)