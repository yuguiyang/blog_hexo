---
title: Pandas手册（6）- 用pandas完成excel中常见任务
date: 2017-08-07 08:00:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***

这里整理下pandas常用的操作，为什么要写这个呢？有本书《利用Python进行数据分析》一边看一遍记录下。

# 1. 重新索引(reindex)
就是重构一下索引，在重构的同时，我们可以做一些其他操作
``` python
DataFrame.reindex(index=None, columns=None, **kwargs)
Conform DataFrame to new index with optional filling logic, placing NA/NaN in locations having no value in the previous index. A new object is produced unless the new index is equivalent to the current one and copy=False

Series.reindex(index=None, **kwargs)
Conform Series to new index with optional filling logic, placing NA/NaN in locations having no value in the previous index. A new object is produced unless the new index is equivalent to the current one and copy=False
```

一个小例子
``` python
obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])

obj
Out[156]: 
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64

#reindex后，没有的值，默认会用NaN填充
obj.reindex(['a','b','c','d','e'])
Out[157]: 
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64

#fill_value，常用的参数，表示没有数据时默认填充的值
obj.reindex(['a','b','c','d','e'] , fill_value=9.9)
Out[159]: 
a   -5.3
b    7.2
c    3.6
d    4.5
e    9.9
dtype: float64


#method,常用参数，在递增或递减index中，填充空值的方法
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])

obj3
Out[165]: 
0      blue
2    purple
4    yellow
dtype: object

obj3.reindex(range(6))
Out[170]: 
0      blue
1       NaN
2    purple
3       NaN
4    yellow
5       NaN
dtype: object

#ffill，前向填充
obj3.reindex(range(6),method='ffill')
Out[167]: 
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object

#bfill，后向填充
obj3.reindex(range(6),method='bfill')
Out[171]: 
0      blue
1    purple
2    purple
3    yellow
4    yellow
5       NaN
dtype: object
```
对于DataFrame来说，用起来也是差不多的

# 2. 丢弃指定轴上的项
主要就是drop方法的使用
``` python
DataFrame.drop(labels, axis=0, level=None, inplace=False, errors='raise')
Return new object with labels in requested axis removed.
```

小例子
``` python
obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])

obj
Out[174]: 
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64

obj.drop('c')
Out[175]: 
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64

obj.drop(['b','d'])
Out[176]: 
a    0.0
c    2.0
e    4.0
dtype: float64


#DataFrame
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                 index=['Ohio', 'Colorado', 'Utah', 'New York'],
                 columns=['one', 'two', 'three', 'four'])

data
Out[178]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

#默认是横轴，
data.drop(['Ohio','Utah'])
Out[179]: 
          one  two  three  four
Colorado    4    5      6     7
New York   12   13     14    15

#我们可以指定axis，在columns上删除
data.drop(['two','four'],axis=1)
Out[180]: 
          one  three
Ohio        0      2
Colorado    4      6
Utah        8     10
New York   12     14
```

# 3. 算术运算和数据对齐
在numpy和pandas中好像都会看到这个词，数据对齐，就是说2个对象在运算的时候，会取一个并集，然后在自动对齐的时候，不重叠的部分就会填充NaN

小例子先看看
``` python
s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])

s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1], index=['a', 'c', 'e', 'f', 'g'])

#index不重叠的地方，会填充NaN
s1+s2
Out[188]: 
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN
dtype: float64

#使用自带的add方法，就可以填充默认值了，这个和我们上面reindex时的思想是一样的
#Series.add(other, level=None, fill_value=None, axis=0)

s1.add(s2,fill_value=0)
Out[189]: 
a    5.2
c    1.1
d    3.4
e    0.0
f    4.0
g    3.1
dtype: float64
```

![](http://upload-images.jianshu.io/upload_images/76024-8c6371911c04ef6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 4.DataFrame和Series之间的运算
这里用到了一个广播的思想，就是指不同形状的数组之间的算术运算的执行方式，很强大的功能，这里，我们先简单了解下。
小例子
``` python
arr = np.arange(12.).reshape((3, 4))

arr
Out[191]: 
array([[  0.,   1.,   2.,   3.],
       [  4.,   5.,   6.,   7.],
       [  8.,   9.,  10.,  11.]])

arr[0]
Out[192]: array([ 0.,  1.,  2.,  3.])

#3行4列的数组，减1行4列的数组，这就是广播
arr - arr[0]
Out[193]: 
array([[ 0.,  0.,  0.,  0.],
       [ 4.,  4.,  4.,  4.],
       [ 8.,  8.,  8.,  8.]])
```

DataFrame和Series之间的计算也是这样
``` python
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list('bde'),
                  index=['Utah', 'Ohio', 'Texas', 'Oregon'])

frame
Out[195]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

s = frame.iloc[0]

s
Out[197]: 
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64

frame - s
Out[198]: 
          b    d    e
Utah    0.0  0.0  0.0
Ohio    3.0  3.0  3.0
Texas   6.0  6.0  6.0
Oregon  9.0  9.0  9.0

s = pd.Series(range(3),index=list('abc'))

frame
Out[223]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

s
Out[224]: 
a    0
b    1
c    2
dtype: int32

frame.add(s)
Out[225]: 
         a     b   c   d   e
Utah   NaN   1.0 NaN NaN NaN
Ohio   NaN   4.0 NaN NaN NaN
Texas  NaN   7.0 NaN NaN NaN
Oregon NaN  10.0 NaN NaN NaN

#我们可以通过axis控制在哪个方向上去广播
frame.add(s,axis=0)
Out[227]: 
         b   d   e
Ohio   NaN NaN NaN
Oregon NaN NaN NaN
Texas  NaN NaN NaN
Utah   NaN NaN NaN
a      NaN NaN NaN
b      NaN NaN NaN
c      NaN NaN NaN
```

在这里，不能使用fill_value填充默认值，还不知道为啥，总是报错，说不支持
![](http://upload-images.jianshu.io/upload_images/76024-db50afbaa0b4bcd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5. 函数应用和映射
这里主要是介绍DataFrame中的一个函数使用，apply，就是对DataFrame中的每一个元素执行传入的函数
``` python
DataFrame.apply(func, axis=0, broadcast=False, raw=False, reduce=None, args=(), **kwds)
Applies function along input axis of DataFrame.
```

小例子
``` python
f = lambda x: x+10

#每一个单元格都会加10
frame.apply(f)
Out[230]: 
           b     d     e
Utah    10.0  11.0  12.0
Ohio    13.0  14.0  15.0
Texas   16.0  17.0  18.0
Oregon  19.0  20.0  21.0


f = lambda x: x.max() - x.min()

frame.apply(f)
Out[232]: 
b    9.0
d    9.0
e    9.0
dtype: float64

#我们可以指定轴，去执行函数
frame.apply(f,axis=1)
Out[233]: 
Utah      2.0
Ohio      2.0
Texas     2.0
Oregon    2.0
dtype: float64
```

这里还有一个applymap函数
``` python
DataFrame.applymap(func)

Apply a function to a DataFrame that is intended to operate elementwise, i.e. like doing map(func, series) for each series in the DataFrame
```

这里得注意下，这2个函数的区别；
目前的理解是，applymap是元素级的，apply在轴上进行操作（貌似不太顺，等明白了再记录下）
``` python
f = lambda x: '${:,.3f}'.format(x)

frame
Out[237]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

#前面，我们有用过，格式化内容的
frame.applymap(f)
Out[238]: 
             b        d        e
Utah    $0.000   $1.000   $2.000
Ohio    $3.000   $4.000   $5.000
Texas   $6.000   $7.000   $8.000
Oregon  $9.000  $10.000  $11.000
```

# 6.处理缺失数据
在pandas中处理缺失数据非常容易，pandas使用浮点值NaN（Not a Number）表示缺失值。
前面，我们说过使用isnull来判断是否有NaN值
小例子
``` python
a = pd.Series(['one','two',np.nan,'three'])

a
Out[240]: 
0      one
1      two
2      NaN
3    three
dtype: object

a.isnull()
Out[241]: 
0    False
1    False
2     True
3    False
dtype: bool

a.notnull()
Out[242]: 
0     True
1     True
2    False
3     True
dtype: bool

#Python内置的None也会被当做NaN处理
a[4]=None

a
Out[247]: 
0      one
1      two
2      NaN
3    three
4     None
dtype: object

a.isnull()
Out[248]: 
0    False
1    False
2     True
3    False
4     True
dtype: bool
```

对于这种数据，我们要怎样处理呢？有的时候，我们可能会初始化为默认值，或者直接剔除掉
我们可以使用dropna函数来剔除掉，或者布尔类型索引
``` python
DataFrame.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)

a
Out[249]: 
0      one
1      two
2      NaN
3    three
4     None
dtype: object

a.dropna()
Out[250]: 
0      one
1      two
3    three
dtype: object

a[a.notnull()]
Out[251]: 
0      one
1      two
3    three
dtype: object

##dataframe

data = pd.DataFrame([[1., 6.5, 3.], [1., np.nan, np.nan],
                  [np.nan, np.nan, np.nan], [np.nan, 6.5, 3.]])

data
Out[254]: 
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0

#默认的话，会将行、列含有NaN的都剔除掉
data.dropna()
Out[255]: 
     0    1    2
0  1.0  6.5  3.0

#我们可以使用参数how来控制
how : {‘any’, ‘all’}

        any : if any NA values are present, drop that label
        all : if all values are NA, drop that label

data.dropna(how='all')
Out[257]: 
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
3  NaN  6.5  3.0
```

有的时候，我们想要做填充而不是剔除，像我们前面使用的参数fill_value
``` python
DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, **kwargs)
Fill NA/NaN values using the specified method
method : {‘backfill’, ‘bfill’, ‘pad’, ‘ffill’, None}, default None

data
Out[261]: 
     0    1    2
0  1.0  6.5  3.0
1  1.0  NaN  NaN
2  NaN  NaN  NaN
3  NaN  6.5  3.0

data.fillna(9.9)
Out[259]: 
     0    1    2
0  1.0  6.5  3.0
1  1.0  9.9  9.9
2  9.9  9.9  9.9
3  9.9  6.5  3.0

#使用method，和前面reindex的时候是一个道理
data.fillna(method='ffill')
Out[262]: 
     0    1    2
0  1.0  6.5  3.0
1  1.0  6.5  3.0
2  1.0  6.5  3.0
3  1.0  6.5  3.0
```