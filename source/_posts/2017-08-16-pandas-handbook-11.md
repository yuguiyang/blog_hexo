---
title: Pandas手册（11）- groupby
date: 2017-08-16 08:00:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***


这里，我们整理下pandas中关于groupby的使用，和SQL中一样，就是对数据进行聚合
可以参考官方：
[http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html#pandas.DataFrame.groupby)
[http://pandas.pydata.org/pandas-docs/stable/groupby.html](http://pandas.pydata.org/pandas-docs/stable/groupby.html)

# 1. groupby基本使用
``` python
DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, group_keys=True, squeeze=False, **kwargs)
Group series using mapper (dict or key function, apply given function to group, return result as series) or by a series of columns.
```
``` python
import pandas as pd
import numpy as np

df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],
                'key2' : ['one', 'two', 'one', 'two', 'one'],
                'data1' : np.random.randint(0,10,5),
                'data2' : np.random.randint(0,10,5)})

df
Out[158]: 
   data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
2      6      5    b  one
3      7      3    b  two
4      7      6    a  one
```

我们现在，根据key1来groupby
``` python
a = df.groupby(by=['key1'])

a
Out[170]: <pandas.core.groupby.DataFrameGroupBy object at 0x000000000B8317F0>
```

我们可以看到，返回值是一个DataFrameGroupBy对象，这只是一个中间数据，还没有进行真正的聚合
这里有一个概念"split-apply-combine"，拆分-应用-合并，感觉和MapReduce的概念差不多，这个的groupby就是做了拆分
我们可以遍历DataFrameGroupBy，
``` python
for k,v in a:
    print('k:',k)
    print('v:',v)
    
k: a
v:    data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
4      7      6    a  one
k: b
v:    data1  data2 key1 key2
2      6      5    b  one
3      7      3    b  two
```

这个就是将内容进行了拆分,当我们在调用统计函数时，才会执行应用和合并
``` python
a.sum()
Out[172]: 
      data1  data2
key1              
a        12     18
b        13      8

a.size()
Out[173]: 
key1
a    3
b    2
dtype: int64

a.count()
Out[174]: 
      data1  data2  key2
key1                    
a         3      3     3
b         2      2     2

a.max()
Out[175]: 
      data1  data2 key2
key1                   
a         7      8  two
b         7      5  two

a.mean()
Out[176]: 
      data1  data2
key1              
a       4.0    6.0
b       6.5    4.0
```

![](http://upload-images.jianshu.io/upload_images/76024-8d628f36b617eb8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以按2个值进行聚合
``` python
df
Out[188]: 
   data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
2      6      5    b  one
3      7      3    b  two
4      7      6    a  one

df.groupby(by=['key1','key2']).sum()
Out[189]: 
           data1  data2
key1 key2              
a    one       9     10
     two       3      8
b    one       6      5
     two       7      3
```

默认的话，会将数值类型的字段做聚合，我们也可以选择
``` python
df.groupby(by=['key1','key2'])['data1'].sum()
Out[190]: 
key1  key2
a     one     9
      two     3
b     one     6
      two     7
Name: data1, dtype: int32

df.groupby(by=['key1','key2'])['data1','data2'].sum()
Out[191]: 
           data1  data2
key1 key2              
a    one       9     10
     two       3      8
b    one       6      5
     two       7      3
```

下面的写法也是同样的，前面我们是直接传入的列名，这里我们传入series也可以
``` python
df.groupby(by=df['key1']).sum()
Out[197]: 
      data1  data2
key1              
a        12     18
b        13      8

df.groupby(by=[df['key1'],df['key2']]).sum()
Out[198]: 
           data1  data2
key1 key2              
a    one       9     10
     two       3      8
b    one       6      5
     two       7      3
```

上面我们传入的都是当前df的序列，这里也可以传入新的，这里只要长度符合就行了，感觉就是把它当成新列来处理
``` python
df.groupby(by=['lufei','lufei','lufei','lufei','namei']).sum()
Out[199]: 
       data1  data2
lufei     18     20
namei      7      6
```

``` python
by : mapping, function, str, or iterable

    Used to determine the groups for the groupby. If by is a function, it’s called on each value of the object’s index. If a dict or Series is passed, the Series or dict VALUES will be used to determine the groups (the Series’ values are first aligned; see .align() method). If an ndarray is passed, the values are used as-is determine the groups. A str or list of strs may be passed to group by the columns in self
```

这个by参数，还可以接收一个dict，像这样：
``` python
df
Out[204]: 
   data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
2      6      5    b  one
3      7      3    b  two
4      7      6    a  one

df.index
Out[205]: RangeIndex(start=0, stop=5, step=1)

df
Out[206]: 
   data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
2      6      5    b  one
3      7      3    b  two
4      7      6    a  one

#默认是根据啊axis=0，所以groupby之前会先将index和dict进行映射，
df.groupby({0:'a',1:'a',2:'a',3:'a',4:'a'}).sum()
Out[207]: 
   data1  data2
a     25     26
```

这里，对于series也是一样的，series也有index，
更厉害的是，这里还可以使用函数进行分组，函数会在各个索引值上调用一次，然后根据返回值来用作分组名称
``` python
df
Out[216]: 
   data1  data2 key1 key2
0      2      4    a  one
1      3      8    a  two
2      6      5    b  one
3      7      3    b  two
4      7      6    a  one

#会把每一个index的值加10，然后再聚合
df.groupby(lambda x:x+10).sum()
Out[217]: 
    data1  data2
10      2      4
11      3      8
12      6      5
13      7      3
14      7      6
```

---------------update at 2017-08-23
这里继续整理下pandas中groupby的使用

# 2. 面向列的多函数应用
 上面，我们再对列做聚合的时候，都是使用使用统一的函数，比如sum(),count(),都是一起的，
在pandas中，我们可以同时调用多个函数，主要是使用agg
``` python
DataFrameGroupBy.agg(arg, *args, **kwargs)
Aggregate using callable, string, dict, or list of string/callables

func : callable, string, dictionary, or list of string/callables

    Function to use for aggregating the data. If a function, must either work when passed a DataFrame or when passed to DataFrame.apply. For a DataFrame, can pass a dict, if the keys are DataFrame column names.

    Accepted Combinations are:

        string function name
        function
        list of functions
        dict of column names -> functions (or list of functions)
```

小栗子
``` python
df = pd.DataFrame({'A': [1, 1, 2, 2],
                    'B': [1, 2, 3, 4],
                    'C': np.random.randn(4)})

df
Out[64]: 
   A  B         C
0  1  1  0.433076
1  1  2  0.509764
2  2  3 -1.091318
3  2  4 -0.696079


df.groupby(by=['A']).min()
Out[65]: 
   B         C
A             
1  1  0.433076
2  3 -1.091318

#使用agg，调用min函数，和直接调用时等价的
df.groupby(by=['A']).agg('min')
Out[66]: 
   B         C
A             
1  1  0.433076
2  3 -1.091318

#还可以传入一个函数数组，同时调用min和max
df.groupby(by=['A']).agg(['min','max'])
Out[67]: 
    B             C          
  min max       min       max
A                            
1   1   2  0.433076  0.509764
2   3   4 -1.091318 -0.696079

df.groupby(by=['A'])['B'].agg(['min','max'])
Out[68]: 
   min  max
A          
1    1    2
2    3    4

#还可以通过传入一个dict，来对不同的列做不同的操作，列名为key，func为value
df.groupby(by=['A']).agg({'B':['min','max'],'C':['sum']})
Out[69]: 
    B             C
  min max       sum
A                  
1   1   2  0.942840
2   3   4 -1.787397
```

上面，我们传入函数，默认会用我们的函数名来做列名，但，有时我们想要自定义，
我们通过传入一个（name,function）的列表
``` python
df.groupby(by=['A']).agg([('the_min_data','min'),('the_max_data','max')])
Out[73]: 
             B                         C             
  the_min_data the_max_data the_min_data the_max_data
A                                                    
1            1            2     0.433076     0.509764
2            3            4    -1.091318    -0.696079

#可以随意组合
df.groupby(by=['A']).agg({'B':['min','max'],'C':[('hey_sum','sum')]})
Out[74]: 
    B             C
  min max   hey_sum
A                  
1   1   2  0.942840
2   3   4 -1.787397
```

# 3. 已无索引形式返回聚合数据
前面，我们groupby之后，都是用聚合建来当做index，我们可以通过参数as_index=False,来取消
``` python
#会默认生成一个新index
df.groupby(by=['A','B'],as_index=False).max()
Out[80]: 
   A  B         C
0  1  1  0.433076
1  1  2  0.509764
2  2  3 -1.091318
3  2  4 -0.696079


df.groupby(by=['A','B'],as_index=True).max()
Out[81]: 
            C
A B          
1 1  0.433076
  2  0.509764
2 3 -1.091318
  4 -0.696079
```

做了练习之后，这里发现，直接调用函数是好用的，但是，如果使用agg来调用，是不好用的这个参数
``` python
df.groupby(by=['A','B'],as_index=False).agg(['min','max'])
Out[79]: 
            C          
          min       max
A B                    
1 1  0.433076  0.433076
  2  0.509764  0.509764
2 3 -1.091318 -1.091318
  4 -0.696079 -0.696079
```