---
title: Pandas手册（8）- 常见绘图
date: 2017-08-12 08:00:00
categories:
- "Python-Pandas"
tags:
- Python
- Pandas
---

Python
Pandas

***

前面，我们大概了解了matplotlib中基本的绘图方式，现在，我们来看看在pandas中绘图的方式，
pandas做好了封装，我们用起来会很方便的。
``` python
Series.plot(kind='line', ax=None, figsize=None, use_index=True, title=None, grid=None, legend=False, style=None, logx=False, logy=False, loglog=False, xticks=None, yticks=None, xlim=None, ylim=None, rot=None, fontsize=None, colormap=None, table=False, yerr=None, xerr=None, label=None, secondary_y=False, **kwds)

#这个kind可以指定图表类型

  ‘line’ : line plot (default)
  ‘bar’ : vertical bar plot
  ‘barh’ : horizontal bar plot
  ‘hist’ : histogram
  ‘box’ : boxplot
  ‘kde’ : Kernel Density Estimation plot
  ‘density’ : same as ‘kde’
  ‘area’ : area plot
  ‘pie’ : pie plot

DataFrame.plot(x=None, y=None, kind='line', ax=None, subplots=False, sharex=None, sharey=False, layout=None, figsize=None, use_index=True, title=None, grid=None, legend=True, style=None, logx=False, logy=False, loglog=False, xticks=None, yticks=None, xlim=None, ylim=None, rot=None, fontsize=None, colormap=None, table=False, yerr=None, xerr=None, secondary_y=False, sort_columns=False, **kwds)
```

# 1. 线形图
``` python
import pandas as pd
import numpy as np

s = pd.Series(np.random.randint(0,100,size=10))

print(s)

s.plot(title='demo-series',label='count',legend=True)
```

![](http://upload-images.jianshu.io/upload_images/76024-0cd44619c18c3018.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.random.randn(10,4)*100,index=np.arange(0,100,10),
                  columns=list('ABCD'))

print(df)

df.plot()
```

![](http://upload-images.jianshu.io/upload_images/76024-acf476c4d976eb77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
DataFrame绘图的时候，会把每一列单独绘制

# 2. 柱状图
``` python
import pandas as pd
import numpy as np

s = pd.Series(np.random.randint(0,100,size=10))

print(s)

s.plot(title='demo-series',label='line',legend=True)
s.plot(kind='bar',colormap='Oranges_r',label='bar',legend=True)
```

我们设置kind='bar'，就可以画柱状图了
![](http://upload-images.jianshu.io/upload_images/76024-369847a4c06ef7f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.DataFrame(np.random.randn(10,4)*100,index=np.arange(0,100,10),
                  columns=list('ABCD'))

print(df)

f,axes = plt.subplots(2,1)
df.plot(kind='bar',ax=axes[0])
df.plot(kind='barh',ax=axes[1])
```

![](http://upload-images.jianshu.io/upload_images/76024-352e41a192195c39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在pandas里画图非常容易，很多都可以是默认转换，index、columns可以自动转换为x轴、y轴标签
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.DataFrame(np.random.rand(6,4)*10,index=['one','two','three','four','five','six'],
                  columns=list('ABCD'))

print(df)

#通过ax参数，可以在不同的subplot上绘图
f,axes = plt.subplots(2,1)
df.plot(kind='bar',ax=axes[0])
df.plot(kind='barh',ax=axes[1])
```

![](http://upload-images.jianshu.io/upload_images/76024-ed173c2b0b5f4629.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在DataFrame中，另一个好用的参数，就是stacked，可以很方便的绘制堆叠图
``` python
df.plot(kind='bar',ax=axes[0],stacked=True)
```

![](http://upload-images.jianshu.io/upload_images/76024-72b533cf816484bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)