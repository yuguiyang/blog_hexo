---
title: 小白学习Tableau-堆叠条按值排序
date: 2017-07-30 20:57:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---
# 序
昨天学习了下堆叠条形图，刚刚看到个类似的教程，说的是，在堆叠条中按值进行排序，挺有意思的。

又学习到了一招，简单分享下。

# 按值在堆叠条中对段进行排序
我们先实现一个简单的堆叠条，

* 使用维度：地区、类别
* 使用度量：销售额

![Tableau-handbook-03-01](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-01.png-blog.photo)

<!-- more -->

下面，我们来看下，是怎样让每个类别的数据块按照销售额排序的。


我们观察下，上面的图，每个地区一个颜色，默认应该是按地区来排序的，每个数据条排序都一样。

我们首先，在颜色“地区”上右键单击，选择“属性”

![Tableau-handbook-03-02](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-02.png-blog.photo)

这里的“维度”、“度量”我都可以理解，但是并没有找到“属性”在Tableau中是怎样定义的，按照以前对BI中维度模型的概念来理解的话，

比如，日期维度，包含3个层级，年、月、日，层级月的属性的话，可能是月份名称、月份中文名称等等。
我们选完属性之后，会变成这个样子

![Tableau-handbook-03-03](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-03.png-blog.photo)

然后，我们选中，维度“类别”和“地区”，创建一个合并字段
![Tableau-handbook-03-04](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-04.png-blog.photo)

这个合并字段，是个什么东西呢？其实就是新增了一个联合维度，就是类别和地区的一个笛卡尔积，内容是这样的
![Tableau-handbook-03-05](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-05.png-blog.photo)
![Tableau-handbook-03-06](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-06.png-blog.photo)

然后，我们把这个合并字段，拖到详细信息上，
![Tableau-handbook-03-07](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-07.png-blog.photo)

然后，我们在合并字段上，选择排序
![Tableau-handbook-03-08](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-08.png-blog.photo)

我们，依次，选择“降序”，“按销售额来”
![Tableau-handbook-03-09](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-09.png-blog.photo)

最后结果，是这样的
![Tableau-handbook-03-10](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-03-10.png-blog.photo)

实例呢，在Tableau Public上，[练习04-堆叠条按值排序](https://public.tableau.com/profile/yuguiyang#!/vizhome/04-_0/sheet2?publish=yes)

最后的话，后面还得详细理解下这里用属性和合并字段一起使用的这个操作，有领悟的话，会在分享。

# 附录

官方教程：[按值在堆叠条中对段进行排序](http://kb.tableau.com/articles/howto/sorting-segments-within-stacked-bars-by-value?lang=zh-cn)