---
title: Kettle手册（九）- 发送邮件 
date: 2017-03-30 11:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---

在Kettle里面，我们每天执行完调度之后，想要监控下JOB的执行状态，通常我们可以会发送邮件，可以的话，还可以发送短信。

在Kettle里面，发送邮件很方便，这里，我们就简单的测试下。

# 1. 在作业中发送简单邮件
我们只需要使用到这个控件就可以了，
![Kettle-handbook-09-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-01.png-blog.photo)
这样，一个简单的发送邮件流程就好了
![Kettle-handbook-09-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-02.png-blog.photo)

<!-- more -->

控件的配置：
收件人，抄送啊，信息，自行填写就行，多个收件人，使用“空格”分隔
![Kettle-handbook-09-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-03.png-blog.photo)
在服务器这里，我们填上服务器的信息就可以了
![Kettle-handbook-09-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-04.png-blog.photo)
这里是邮件消息的一些配置，
![Kettle-handbook-09-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-05.png-blog.photo)
暂时先到这里，我们测试下结果
![Kettle-handbook-09-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-06.png-blog.photo)
然后，查看邮箱，我们会接收到这个邮件，刚刚简单测了下这个“回复名称”，就是
![Kettle-handbook-09-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-07.png-blog.photo)
![Kettle-handbook-09-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-08.png-blog.photo)
这里试过中文，会有问题，有乱码，可能是Windows下的原因，没有再去测试验证
![Kettle-handbook-09-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-09.png-blog.photo)
就是收到邮件时的一个发件人的名称，不同邮箱显示的不一样

# 2. 增加附件
附件的话，也很简单，上面的面板中直接配置就可以了
![Kettle-handbook-09-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-10.png-blog.photo)
然后，我们需要将待发送的邮件，添加到结果集中
![Kettle-handbook-09-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-11.png-blog.photo)
在控件中，我们添加好文件就行了。
![Kettle-handbook-09-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-12.png-blog.photo)
![Kettle-handbook-09-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-13.png-blog.photo)
我们再次发送，验证下
![Kettle-handbook-09-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-14.png-blog.photo)
好了，附件也可以了，思路就是这样的，实际应用时，可能还有些问题得注意下

# 3. 自定义邮件内容

到这里，我们会看到，邮件的正文内容，可能并不是我们想要的， 我们想要的可能是这样的信息
![Kettle-handbook-09-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-15.png-blog.photo)
这就需要自定义正文内容，我们需要勾选下面这个选项
![Kettle-handbook-09-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-16.png-blog.photo)
这里是可以使用变量的，我们可以拼接HTML来实现
![Kettle-handbook-09-17.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-17.png-blog.photo)
![Kettle-handbook-09-18.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-09-18.png-blog.photo)
好了，邮件的介绍，大概就这些，在转换中，也是可以使用的，大同小异

