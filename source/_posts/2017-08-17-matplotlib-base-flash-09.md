---
title: matplotlib手册(9) - 绘制动画
date: 2017-08-17 11:11:00
categories:
- "数据可视化-Python&R"
tags:
- 数据可视化
- matplotlib
- Python
---
matplotlib手册(9)

> 绘制动画 

前面，我们介绍了很多绘图的方法，matplotlib不单单可以绘制静态的图，还可以制作动态的图，下面，我们就来学习下。

> 我们主要参考matplotlib官网的例子[http://matplotlib.org/api/animation_api.html](http://matplotlib.org/api/animation_api.html)

创建动画最简单的方式，就是使用Animation的子类,就是下面的这2个

# 1. FuncAnimation
函数介绍及主要参数
``` python
class matplotlib.animation.FuncAnimation(fig, func, frames=None, init_func=None, fargs=None, save_count=None, **kwargs)

fig : matplotlib.figure.Figure
func : callable
frames : iterable, int, generator function, or None, optional
init_func : callable, optional
fargs : tuple or None, optional
interval : number, optional

repeat_delay : number, optional
repeat : bool, optional
```
小栗子
``` python
# -*- coding: utf-8 -*-
"""
Created on Thu Aug 17 18:19:08 2017

@author: yuguiyang
"""

import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib.animation as animation 
from datetime import datetime 
  

fig,axes = plt.subplots()  
axes.plot(np.random.rand(10))  

#重新绘制图形
def update_line(data): 
    print(datetime.now(),'--',data)
    #清空当前轴
    plt.cla()
    #重新绘图
    axes.plot(np.random.rand(10))


#传入的fig中，调用update_line函数，将range(3)作为参数传给update_line，1秒调用一次
ani = animation.FuncAnimation(fig, update_line, 3, interval=1000)  

plt.show() 
```
上面的代码，我们定义了一个函数update_line，他会清空axes，并重新绘图；
FuncAnimation会每个1秒调用一次这个函数
![matplotlib-base-flash-09-01](http://7xl61k.com1.z0.glb.clouddn.com/matplotlib-base-flash-09-01.gif-blog.photo)

这里记录一个小问题，暂时还没有解决
## frames 参数的问题
上面的例子里，我们给的是一个常量3，按照官方的介绍，是按照range(3),来一次传给函数的，但实际测试下来，发现他的调用会有问题。
我们看下上面的那个输出

![matplotlib-base-flash-09-03](http://7xl61k.com1.z0.glb.clouddn.com/matplotlib-base-flash-09-03.png-blog.photo)


***
刚刚发现了导致这个问题的原因，注意看这个：
``` python
init_func : callable, optional

    A function used to draw a clear frame. If not given, the results of drawing from the first item in the frames sequence will be used. This function will be called once before the first frame.
```
上面，因为我们没有指定初始化函数，所以导致，会调用一次update_line，用它返回值作为初始状态，我们改下脚本再看
``` python
def init():
    print('init')
    #清空当前轴
    plt.cla()
    
ani = animation.FuncAnimation(fig, update_line, 3, repeat=False, interval=3000,init_func=init)  
    
```
这回输出就正常了，

![matplotlib-base-flash-09-04](http://7xl61k.com1.z0.glb.clouddn.com/matplotlib-base-flash-09-04.png-blog.photo)

## repeat、repeat_delay
这2个参数一般会配合使用，repeat默认是true，所以上面的例子会一直循环下去，
如果我们改为false,第一次循环完之后就会停止。


# 2. ArtistAnimation
``` python
class matplotlib.animation.ArtistAnimation(fig, artists, *args, **kwargs)

artists : list

    Each list entry a collection of artists that represent what needs to be enabled on each frame. These will be disabled for other frames.

```
使用起来和上面的差不多，这里不会调用函数，而是传入一个list，
``` python
import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib.animation as animation 
  

fig,axes = plt.subplots()  
    
ims= []
for i in range(5):
    ims.append(axes.plot(np.random.rand(10)))

im_ani = animation.ArtistAnimation(fig, ims, interval=1000, repeat_delay=3000,
                                   blit=True)
print(ims)
plt.show() 
```

![matplotlib-base-flash-09-05](http://7xl61k.com1.z0.glb.clouddn.com/matplotlib-base-flash-09-05.png-blog.photo)
