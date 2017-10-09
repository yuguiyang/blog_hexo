---
title: Pandas手册（4）- 对数据进行筛选和排序
date: 2017-07-31 21:59:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***
前几天看了篇教程：[使用Pandas对数据进行筛选和排序](http://bluewhale.cc/2016-08-06/use-pandas-filter-and-sort.html)

里面主要介绍了，我们在使用Pandas时，对数据进行筛选和排序的介绍
这里简单总结分享下自己。

# 排序
可能是版本的问题，原文中的sort函数没有了，变成了2个常用的函数 sort_index和sort_value

``` python
DataFrame.sort_index(axis=0, level=None, ascending=True, inplace=False, kind='quicksort', na_position='last', sort_remaining=True, by=None)
Sort object by labels (along an axis)

DataFrame.sort_values(by, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last')
Sort by the values along either axis
```

sort_index：按照索引排序，及列标签或行标签，axis=0是列标签，axis=1是行标签
``` python
df3 = pd.DataFrame(np.random.randn(6,4),
                   index=list(range(0,12,2)),
                   columns=list(range(0,8,2)))

print(df3)
print(df3.sort_index(axis=0,ascending=False))
print(df3.sort_index(axis=1,ascending=False))
```

![](http://upload-images.jianshu.io/upload_images/76024-66d497e63d83a214.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

sort_value：按值进行排序，这个估计用的会多些，按数据内容进行排序
``` python
print(df3)
print(df3.sort_values(by=[0],axis=0,ascending=True))
print(df3.sort_values(by=[2],axis=0,ascending=False))
```

![](http://upload-images.jianshu.io/upload_images/76024-2b887473b56dc8d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

排序后呢，我们可能有，只需要看到前10、后10这类的需求，需要用到另一个函数 head()、tail()

# 筛选
筛选呢，我们上一篇介绍了loc和iloc
[Pandas手册（3）-DataFrame-Selection By Label/Position ](https://ask.hellobi.com/blog/yuguiyang1990/9093)

这里，我们实际应用下
``` python
import pandas as pd
import numpy as np

df = pd.read_excel(r'D:\document\tableau_data\data_stu.xlsx',sheetname=0)
print(df)

#按照数学、语文，降序排列
print(df.sort_values(by=['数学','语文'],ascending=False))
#按照数学、语文，降序排列，取前3
print(df.sort_values(by=['数学','语文'],ascending=False).head(3))
#按照数学、语文，降序排列，取后3
print(df.sort_values(by=['数学','语文'],ascending=False).tail(3))

#筛选数学大于90的
print(df.loc[df['数学']>=90])
#筛选数学大于等于90，且语文小于60的
print(df.loc[(df['数学']>=90) & (df['语文']<60)])
#筛选数学或语文大于60分，爱英语排序
print(df.loc[(df['数学']>=60) | (df['语文']>=60)].sort_values(by=['英语']))
```

数据如下，
![image.png](http://upload-images.jianshu.io/upload_images/76024-126c2bf3c2da4b8b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们主要是多种过滤条件的整合使用，大家多练习就可以掌握了

# 附录（参考资料）
[使用Pandas对数据进行筛选和排序 ](http://bluewhale.cc/2016-08-06/use-pandas-filter-and-sort.html)