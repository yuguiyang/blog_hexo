---
title: Pandas手册（10）- 数据转换
date: 2017-08-15 08:00:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***

这里接着上一篇，继续记录下pandas中数据处理方面的函数

# 1.  重复数据
结果集中，可能会有重复数据，有函数可以做去重操作

``` python
#判断数据是否重复
DataFrame.duplicated(subset=None, keep='first')
Return boolean Series denoting duplicate rows, optionally only considering certain columns

#删除重复记录
DataFrame.drop_duplicates(subset=None, keep='first', inplace=False)
Return DataFrame with duplicate rows removed, optionally only considering certain columns
```
``` python
data = pd.DataFrame({'K1':['one']*3+['two']*4,'k2':[1,1,2,3,3,4,4]})

data
Out[100]: 
    K1  k2
0  one   1
1  one   1
2  one   2
3  two   3
4  two   3
5  two   4
6  two   4

#keep参数，默认是first，会以第一个为准，后面再出现就是True
data.duplicated()
Out[103]: 
0    False
1     True
2    False
3    False
4     True
5    False
6     True
dtype: bool

data.duplicated(keep='last')
Out[116]: 
0     True
1    False
2    False
3     True
4    False
5     True
6    False
dtype: bool

#这里drop也是一样的，可以用keep控制保留第一次出现的，还是最后1次出现的
data.drop_duplicates()
Out[104]: 
    K1  k2
0  one   1
2  one   2
3  two   3
5  two   4
```

这2个函数，默认都是判断全部的列，也可以指定列
``` python
data['v1']=range(7)

data
Out[118]: 
    K1  k2  v1
0  one   1   0
1  one   1   1
2  one   2   2
3  two   3   3
4  two   3   4
5  two   4   5
6  two   4   6

data.duplicated()
Out[119]: 
0    False
1    False
2    False
3    False
4    False
5    False
6    False
dtype: bool

data.duplicated(subset=['K1'])
Out[120]: 
0    False
1     True
2     True
3    False
4     True
5     True
6     True
dtype: bool

data.drop_duplicates(subset=['K1'])
Out[121]: 
    K1  k2  v1
0  one   1   0
3  two   3   3
```

# 2. 利用函数或映射进行数据转换
有的时候，我们想要对Series和dataframe中的每个元素做些转换，就用到了这个函数，和Python内置的map函数类似
``` python
Series.map(arg, na_action=None)
Map values of Series using input correspondence (which can be a dict, Series, or function)
arg : function, dict, or Series

DataFrame.applymap(func)
Apply a function to a DataFrame that is intended to operate elementwise, i.e. like doing map(func, series) for each series in the DataFrame
```

``` python
s1 = pd.Series(range(10))

s1
Out[133]: 
0    0
1    1
2    2
3    3
4    4
5    5
6    6
7    7
8    8
9    9
dtype: int32

s1.map(lambda x:x+10)
Out[134]: 
0    10
1    11
2    12
3    13
4    14
5    15
6    16
7    17
8    18
9    19
dtype: int64

s1.map(lambda x:x*-1)
Out[135]: 
0    0
1   -1
2   -2
3   -3
4   -4
5   -5
6   -6
7   -7
8   -8
9   -9
dtype: int64
```

这个map，不单单可以是函数，还可以是 Series或dict
``` python
x = pd.Series([1,2,3], index=['one', 'two', 'three'])

y = pd.Series(['foo', 'bar', 'baz'], index=[1,2,3])

x
Out[142]: 
one      1
two      2
three    3
dtype: int64

y
Out[143]: 
1    foo
2    bar
3    baz
dtype: object

x.map(y)
Out[144]: 
one      foo
two      bar
three    baz
dtype: object

z = {1: 'A', 2: 'B', 3: 'C'}

x.map(z)
Out[146]: 
one      A
two      B
three    C
dtype: object
```

# 3.替换值
在字符串中，我们经常会用到replace函数，用来替换指定的值，pandas中也有
``` python
DataFrame.replace(to_replace=None, value=None, inplace=False, limit=None, regex=False, method='pad', axis=None)
Replace values given in ‘to_replace’ with ‘value’.
to_replace : str, regex, list, dict, Series, numeric, or None

Series.replace(to_replace=None, value=None, inplace=False, limit=None, regex=False, method='pad', axis=None)
Replace values given in ‘to_replace’ with ‘value’.
```

``` python
data = pd.Series([1., -999., 2., -999., -1000., 3.])

data
Out[150]: 
0       1.0
1    -999.0
2       2.0
3    -999.0
4   -1000.0
5       3.0
dtype: float64

#我们将-999这个异常值替换掉
data.replace(-999,np.nan)
Out[151]: 
0       1.0
1       NaN
2       2.0
3       NaN
4   -1000.0
5       3.0
dtype: float64

#同时替换多个值，也可以
data.replace([-999,-1000],np.nan)
Out[152]: 
0    1.0
1    NaN
2    2.0
3    NaN
4    NaN
5    3.0
dtype: float64

data.replace([-999,-1000],[np.nan,0])
Out[153]: 
0    1.0
1    NaN
2    2.0
3    NaN
4    0.0
5    3.0
dtype: float64
```