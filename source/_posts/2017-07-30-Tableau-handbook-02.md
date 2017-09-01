---
title: 小白学习Tableau-堆叠条形图
date: 2017-07-30 19:57:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---

# 序
昨天在看水足迹那个可视化题目的时候，就想做一个堆叠条形图，但是发现只有一个维度，怎么也拖不出来，后来改了下数据源，成功实现了。今天搜到个例子，发现了解决办法，只能说明，还是对Tableau不熟啊，没有能领悟Tableau的内涵。

# 堆叠条形图
教程中介绍了2种方法，我们都来实践一下。这里面，最大的收获，就是原来那个“度量名称”和“度量值”是可以拖拽过去使用的，我也是醉了，这个操作得好好研究下，理解下Tableau的机制。

这里的度量名称，应该就是所有度量的一个集合；度量值应该就是“度量名称”集合中选定的度量。

这里我们就用 [http://www.makeovermonday.co.uk/](http://www.makeovermonday.co.uk/) 上的数据

![Tableau-handbook-02-01](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-01.png-blog.photo)

<!-- more -->

## 对每个维度上使用一个单独的条
首先，我们将“食物”维度，拖到列上，
![Tableau-handbook-02-02](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-02.png-blog.photo)

然后，我们需要观察的是3种水足迹的值，并让他们显示成堆叠图的样子

我们把度量名称，拖到颜色上
![Tableau-handbook-02-03](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-03.png-blog.photo)

因为，我们只看3个水足迹的量，我们就筛选下
![Tableau-handbook-02-04](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-04.png-blog.photo)
![Tableau-handbook-02-05](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-05.png-blog.photo)
![Tableau-handbook-02-06](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-06.png-blog.photo)


（这个颜色是我之前设置过的，大家操作完，颜色可能和我不一样，但样式是一样的）
就这样，我们完成了，列上是每一种食物，然后行上面是选定的3中水足迹，一个简单的堆叠图就完成了。

## 对每个度量使用一个单独的条

这个操作其实就是和上面的操作的相反我们将度量名称拖到列上，并筛选
![Tableau-handbook-02-07](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-07.png-blog.photo)
![Tableau-handbook-02-08](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-08.png-blog.photo)
![Tableau-handbook-02-09](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-02-09.png-blog.photo)

然后，将度量值拖到行上

最后，将维度，食物拖到颜色上

这样，我们就在3个度量值上，看食物维度的一个堆积情况。
好了，堆积图的例子就完成了，主要是理解下Tableau的思想。

小例子，发布在Tableau Public上：[练习03-堆叠条形图](https://public.tableau.com/profile/yuguiyang#!/vizhome/03-/sheet2?publish=yes)

# 附录（参考资料）
官方教程：[使用多个度量创建堆叠条形图](http://kb.tableau.com/articles/howto/stacked-bar-chart-multiple-measures?lang=zh-cn)