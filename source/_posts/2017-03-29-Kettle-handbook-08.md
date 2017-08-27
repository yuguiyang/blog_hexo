---
title: Kettle手册（八）- 循环 
date: 2017-03-29 23:40:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
有的时候，我们想要在Kettle中实现这个循环的功能，比如，批量加载数据的时候，我们要对10张表执行同样的操作，只有表名和一些信息不一样，这时，写个循环就省事儿多了

# 1. 遍历结果集实现
![Kettle-handbook-08-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-01.png-blog.photo)
这里的话，我们主要是通过一个将结果集返回，然后通过转换的设置来实现的

<!-- more -->

## 1.1 query_the_result
这个转换，只要是将我们要遍历的结果集返回，
![Kettle-handbook-08-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-02.png-blog.photo)
表输入，我们就是返回了5条记录，来做遍历
![Kettle-handbook-08-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-03.png-blog.photo)
复制记录到结果，这个控件的作用，就是我们可以在后面的转换继续使用这个结果集。

##1.2 traverse_the_result
这里呢，我们就是需要遍历的转换了，这里，我们只是获取结果集，然后将结果集输出
![Kettle-handbook-08-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-04.png-blog.photo)
![Kettle-handbook-08-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-05.png-blog.photo)
还有一个很重要的一步，怎样让这个转换可以根据结果集的条数，去循环执行呢？
就是这个“执行每一个输入行”
![Kettle-handbook-08-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-06.png-blog.photo)
我们执行下看看
![Kettle-handbook-08-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-07.png-blog.photo)

# 2. 使用JS实现
网上有很多的例子，介绍怎样用JS来控制循环，这里我们也简单的测试下
![Kettle-handbook-08-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-08.png-blog.photo)

## 2.1 query_the_result
这一步，和上面的一样，就是将结果集返回
![Kettle-handbook-08-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-09.png-blog.photo)

## 2.2 travers_the_result
这里主要是使用JS将结果集进行遍历，通过JS，将一些结果存放到变量里面，在后面的操作中就可以使用了，通过${xxx}的方式使用
这个其实和Java、JS里面循环思路一样，通过结果集的总数“total_num”和下标“LoopCounter”进行判断
![Kettle-handbook-08-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-10.png-blog.photo)

## 2.3 evaluate_the_loop_count
这一步，就是判断下标的值和结果集的总数，进行对比，
![Kettle-handbook-08-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-11.png-blog.photo)

## 2.4 print_the_log
输出下，我们想要使用的变量
![Kettle-handbook-08-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-12.png-blog.photo)

## 2.5 manage_the_loop_index
这一步，给下标加一，然后获取下一条记录
![Kettle-handbook-08-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-13.png-blog.photo)
好了，执行下，我们看看
![Kettle-handbook-08-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-08-14.png-blog.photo)
好了，循环的使用先介绍到这里
