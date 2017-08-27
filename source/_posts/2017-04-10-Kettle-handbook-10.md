---
title: Kettle手册（十）- 跨库查询
date: 2017-04-10 11:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
Kettle整体使用起来，还是很方便的，熟悉应用了之后，就是对控件的熟悉和使用了，只要思路有了，就是整合下Kettle中各个控件的使用就行。
这里，简单介绍下一个“跨库查询”的控件。
有的时候，我们一个脚本，可能只是临时性的，或者需要实时的去查一下，同步到数仓的话，可能不太方便，我们就可以使用跨库查询的控件
用到的表信息
![Kettle-handbook-10-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-01.png-blog.photo)
![Kettle-handbook-10-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-02.png-blog.photo)

<!-- more -->

# 1. 数据库连接(Database Join)
我们先用这个控件来实现一下
![Kettle-handbook-10-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-03.png-blog.photo)
用起来也很简单
![Kettle-handbook-10-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-04.png-blog.photo)
表输入：是我们第一个库中的SQL
数据库连接：是我们另一个库的SQL
![Kettle-handbook-10-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-05.png-blog.photo)
我们用关联的字段放在where条件后，使用“?”来占位，并在下面，选择要传入的参数
默认的话，是JOIN，我们也可以勾选Outer Join，
然后，我们看下，输出就行
![Kettle-handbook-10-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-06.png-blog.photo)
这是后面导出的文件，
![Kettle-handbook-10-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-07.png-blog.photo)
这里，我们就简单实现了跨库的查询

# 2. 数据库查询
我们再来看另一个控件，“数据库查询”，这个控件同样可以实现跨库，但是有一个小问题
首先，我们使用上一次的数据来看
![Kettle-handbook-10-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-08.png-blog.photo)
![Kettle-handbook-10-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-09.png-blog.photo)
我们执行下，结果看上去是一样的
![Kettle-handbook-10-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-10.png-blog.photo)
这其实有个隐藏的问题，我们再增加几条记录看看
![Kettle-handbook-10-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-11.png-blog.photo)
比如：现在1号有2条记录，正常的话，我们导出也是要有2条的
我们执行下看看
![Kettle-handbook-10-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-12.png-blog.photo)
我们会看到，数据并没有增加，这是控件导致的，
先获取左边的结果集，然后一条一条去右边匹配；匹配到第一条记录后，就会跳出，直接去匹配下一个，所以，我们有2条记录，也只会找到第一个。
这并不是我们想要的，我们再试下第一个控件
![Kettle-handbook-10-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-13.png-blog.photo)
使用这个“数据库查询”控件的话，可以通过将1-N关系汇总，将N的一方，放在前面
![Kettle-handbook-10-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-14.png-blog.photo)
![Kettle-handbook-10-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-15.png-blog.photo)
最后的结果也是可以的
![Kettle-handbook-10-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-10-16.png-blog.photo)

