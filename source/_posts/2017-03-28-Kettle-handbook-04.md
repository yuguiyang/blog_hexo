---
title: Kettle手册（四）- 变量的使用
date: 2017-03-28 23:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
我们在这一回，介绍下，Kettle中全局变量的使用，我们前面说过的配置文件，其实就是配置全局变量的地方
[Kettle手册（三）- 配置文件的使用及密码加密 ](2017/03/27/Kettle-handbook-03/)


{% note primary %} 
## 1. 全局变量
{% endnote %}

就是我们上面说的kettle.properties文件，我们在里面定义的变量，我们可以在所有的转换或者作业中获得到，比如，我们前面，说的数据库参数
![Kettle-handbook-04-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-01.png-blog.photo)
之前，我们已经在数据库连接中测试过，是可以，这里，我们输出下这个变量，看看

<!-- more -->

### 1.1 输出变量的值
我们这里，用到了“获取变量"这个控件
![Kettle-handbook-04-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-02.png-blog.photo)
我们单击，"Get Variables",就可以获取到当前的全局变量信息
![Kettle-handbook-04-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-03.png-blog.photo)
我们选择几个输出试试
![Kettle-handbook-04-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-04.png-blog.photo)
还有一个，”日志“控件，
![Kettle-handbook-04-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-05.png-blog.photo)
拖好之后，我们直接执行，
![Kettle-handbook-04-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-06.png-blog.photo)
日志中，我们会看到，我们定义在文件中的参数（加密的参数，我没有重启，所以显示的还是原来的）
那我们，可不可以，动态的增加变量呢？

### 1.2 动态增加变量
刚刚也在网上找了些资料，尝试了下，这里简单分享下（貌似，这得算是对局部变量的操作，暂时就放在这里吧）
我们先试下在转换中设置变量，作业中也是可以使用的，我们后面再说
测试流程是这样的， 我们再表输入中，有2个时间参数，然后作为变量
![Kettle-handbook-04-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-07.png-blog.photo)
比如，有这样一个场景，我们每天需要定时调度一些SP，SP都有开始时间，结束时间，调用时，需要传参数进去，
这个时候，我们在使用Kettle的时候，就可以通过这样的方式，去设置变量，然后再调用SP
![Kettle-handbook-04-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-08.png-blog.photo)
我们单击获取字段后，就可以了，这里可以修改变量存在的范围
![Kettle-handbook-04-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-09.png-blog.photo)
![Kettle-handbook-04-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-10.png-blog.photo)
执行后，输出，后面，我们就可以使用这2个时间变量了
![Kettle-handbook-04-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-11.png-blog.photo)
这里使用的时候，也遇到一个问题，就是变量的默认值，一直都没有生效，不知道为什么，不管是，静态值，还是变量值，都没有办法，待研究。


{% note primary %} 
## 2. 局部变量（命名参数）
{% endnote %}

在kettle中，相对于全局变量，我们还可以使用局部变量。感觉，这个全局变量，局部变量，都是相对而言的，
就网上大部分资料来说，Kettle中的局部变量就是“命名参数”
我们再转换中，右键单击，选择，转换设置

![Kettle-handbook-04-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-12.png-blog.photo)
我们选择，“命名参数”，定义一个变量，我们给一个默认值
![Kettle-handbook-04-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-13.png-blog.photo)
然后，在日志中，将变量输出
![Kettle-handbook-04-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-14.png-blog.photo)
我们执行下，这个转换，运行时的界面，我们可以看到，这个参数是可以动态改变的，或者，我们再命令行调这个转换的时候，同样可以给他赋值
![Kettle-handbook-04-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-15.png-blog.photo)
运行结果，这个就是简单的局部变量了
![Kettle-handbook-04-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-04-16.png-blog.photo)








