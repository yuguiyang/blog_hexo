---
title: Kettle手册（六）- Hop小记
date: 2017-03-29 22:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
# 1. 什么是Hop
在我们前面，使用Kettle过程中，控件与控件之间的连线，这里，我们详细介绍下它，它在Kettle中叫Hop（跳）。
![Kettle-handbook-06-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-01.png-blog.photo)
![Kettle-handbook-06-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-02.png-blog.photo)

<!-- more -->

# 2. Hop的发送方式（转换）
在转换中，一般情况，控件和控件之间只有一个Hop，当然，如果需要的话，我们拖了2个控件出来，像这样：
![Kettle-handbook-06-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-03.png-blog.photo)
Kettle会提示你，下面的信息，让你选择，数据发送的方式
![Kettle-handbook-06-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-04.png-blog.photo)

## 2.1 分发记录
目标步骤轮流接收记录，其实就是你一条，我一条，轮着接收数据，这个我们试一下就知道了，
![Kettle-handbook-06-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-05.png-blog.photo)
![Kettle-handbook-06-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-06.png-blog.photo)
我们执行下，看看这个结果试试，我们再步骤度量中，可以看到，a.txt和b.txt分别写入的数量
![Kettle-handbook-06-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-07.png-blog.photo)
看看结果文件，就是这样的
![Kettle-handbook-06-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-08.png-blog.photo)

## 2.2 复制记录
所有记录同时发送到所有的目标步骤，这个看起来就简单多了，比如上面的例子，2个文本文件会接收到同样的所有的数据，我们也试一下
![Kettle-handbook-06-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-09.png-blog.photo)
结果文件的话，就是2个节点，接收到的数据都是一样的
![Kettle-handbook-06-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-10.png-blog.photo)

# 3.Hop的状态（作业）
在作业中，Hop主要用来控制流程
![Kettle-handbook-06-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-11.png-blog.photo)
有3种状态，一个锁，一个绿色的对号，一个红色的叉号
![Kettle-handbook-06-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-12.png-blog.photo)
![Kettle-handbook-06-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-13.png-blog.photo)
![Kettle-handbook-06-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-14.png-blog.photo)
简单来说，
![Kettle-handbook-06-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-15.png-blog.photo)：表示无论上一步执行成功还是失败，都一定会执行下一步
![Kettle-handbook-06-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-16.png-blog.photo)：表示上一步执行成功才会执行下一步
![Kettle-handbook-06-17.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-06-17.png-blog.photo)：表示上一步执行失败执行下一步
比如我们上面的例子，我们的转换执行成功后，就结束了，如果转换执行失败了，我们就发送邮件。