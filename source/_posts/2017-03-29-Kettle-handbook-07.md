---
title: Kettle手册（七）- 资源库的使用
date: 2017-03-29 23:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
# 1. 为什么使用资源库
之前，我们新建转换或者作业的时候，都是直接保存在本地，而如果我们是多人开发的话，除了使用SVN等版本控制软件，还可以使用Kettle的资源库，他会将转换、作业直接保存在数据库中，而且，连接资源库的话，我们就不需要每一次都新建数据库连接了，用起来还是蛮方便的。

# 2. 新建资源库
Kettle7.0里面，是在右上角这个Connect来连接的
![Kettle-handbook-07-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-01.png-blog.photo)

<!-- more -->

## 2.1 资源库的类型
资源库有3中类型
Pentaho Repository
Database Repository（使用数据库存储）
File Repository（使用文件存储）
![Kettle-handbook-07-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-02.png-blog.photo)
![Kettle-handbook-07-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-03.png-blog.photo)

## 2.2 新建Pentaho Repository
我们单击上面的get started 之后，就会进入新建界面
![Kettle-handbook-07-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-04.png-blog.photo)
[http://localhost:8080/pentaho](http://localhost:8080/pentaho)
一开始还没搞懂这个Server到底怎么启动，后来google了半天发现
![Kettle-handbook-07-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-05.png-blog.photo)
后来又找到了这个，应该是要安装其他的组件才行，这个类型的库就放弃吧。。
![Kettle-handbook-07-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-06.png-blog.photo)

## 2.3 Database Repository
好了，这回，我们选择哪个database的资源库
![Kettle-handbook-07-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-07.png-blog.photo)
我们填一个connection的名字，然后配置一个资源库的连接就可以了，最好给kettle新建一个数据库使用
![Kettle-handbook-07-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-08.png-blog.photo)
至于数据库连接的话，和转换里面是一样的，大家可以自行新建一个
![Kettle-handbook-07-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-09.png-blog.photo)
配置好，以后，大家选择Finish就可以了，然后，我们可以连接下这个库，注意下，这里的用户名和密码，
默认是admin/admin，大家直接登录就好了，这是Kettle自己初始化的
这个怎么改呢，暂时还没有发现，待研究，等我再google看看，估计官网上会有。
（找了下，发现了在哪改密码，就是刚刚的搜索资源库
![Kettle-handbook-07-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-10.png-blog.photo)
)
![Kettle-handbook-07-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-11.png-blog.photo)
连接后，我们正常使用就好了，没啥两样，会多一些功能，比如，探索资源库这里
![Kettle-handbook-07-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-12.png-blog.photo)
我们再保存作业和转换的话，会直接保存在数据库中，
![Kettle-handbook-07-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-13.png-blog.photo)
而且，很好的一个功能，个人感觉，就是数据库连接只需要创建一次，在哪里都可以用了，不需要再次创建。

## 2.4 File Repository
这个和database的资源库，就差不多了，只不过是基于文件的，保存在本地就可以了
![Kettle-handbook-07-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-14.png-blog.photo)
这个就和Eclipse一个工作区差不多，转换、作业都保存在这个目录下
![Kettle-handbook-07-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-07-15.png-blog.photo)
好了，关于资源库，就简单的说这些了，大家可以自行连接，试试。
