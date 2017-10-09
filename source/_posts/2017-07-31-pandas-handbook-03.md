---
title: Pandas手册（3）- DataFrame-Selection By Label/Position
date: 2017-07-31 14:59:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***
## 序
这里主要介绍下，在DataFrame中一些筛选的操作，常用的有下面这些
![](http://upload-images.jianshu.io/upload_images/76024-b0e3643a3e1474d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

熟练掌握上面的几个方法，操作DataFrame应该就足够了
``` python
import pandas as pd
import numpy as np

d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
     'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}

df = pd.DataFrame(d)

print('原始数据：\n',df)

print('index 为a的数据：\n',df.loc['a'])
print('index下标为2的数据：\n', df.iloc[2])
```

![](http://upload-images.jianshu.io/upload_images/76024-b0d90b7a325bbccf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

## .loc函数
，主要就是通过label来获取row数据
![](http://upload-images.jianshu.io/upload_images/76024-868722f8a3c65ef9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

前面的例子，都是通过label来输出指定的行数据，其实也可以控制输出指定的列
``` python
df2 = pd.DataFrame(np.random.randn(6,4),index=list('abcdef'),
                   columns=list('ABCD'))
print(df2)
print(df2.loc['c':])
print(df2.loc['d':,['A','D']])
```

![](http://upload-images.jianshu.io/upload_images/76024-736ad70ce4b40f5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们还可以实现更复杂的筛选
我们只输出指定的列，label 为a的行数值大于-1且小于0的列
``` python
print(df2.loc[:,(df2.loc['a']>-1) & (df2.loc['a']<0)])
```

![](http://upload-images.jianshu.io/upload_images/76024-e8e2d1d4fffed62d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

\#输出指定单元格数据print(df2.loc['a','C'])

![](http://upload-images.jianshu.io/upload_images/76024-7f2df349c45178e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## .iloc函数
就是通过下标来筛选数据
![](http://upload-images.jianshu.io/upload_images/76024-8e5c57342a85b6c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
df3 = pd.DataFrame(np.random.randn(6,4),
                   index=list(range(0,12,2)),
                   columns=list(range(0,8,2)))
print(df3)
#输出第2行
print(df3.iloc[1])
print(df3.iloc[:3])
print(df3.iloc[3:5,1:3])
print(df3.iloc[[1, 3, 5], [1, 3]])
```

![](http://upload-images.jianshu.io/upload_images/76024-e3f81ec5b22abde0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 附录（参考资料）
[Indexing and Selecting Data](http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing)
[Selection By Label](http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-label)