---
title: Kettle手册（十一）- 用PGP加密、加密文件
date: 2017-04-11 11:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
看到有同学提问，以前也没用过，百度了一下，找了些资料，这里记录下。

# 1. 安装gpg4win
这个gpg4win是干嘛的呢，我们可以去他的官网看看：[gpg4win](https://www.gpg4win.org/index.html)
目前，只知道他是加密的，这个是对Windows平台使用的
![Kettle-handbook-11-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-01.png-blog.photo)
这里可能还有个PGP的概念，看看百度百科
![Kettle-handbook-11-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-02.png-blog.photo)

<!-- more -->

好了，具体概念，大家可以自行找找，我们下载下来，然后安装一下即可
![Kettle-handbook-11-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-03.png-blog.photo)
这个是昨天安装的，就不粘贴步骤了，安装完后，我们要先创建一个证书的东西，我们打开这个管理界面
![Kettle-handbook-11-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-04.png-blog.photo)
打开后，是这样一个界面，（网上有这个的安装配置教程，这里也简单介绍下，不清楚的可以再百度看看）
![Kettle-handbook-11-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-05.png-blog.photo)
我们新建一个Certificate
![Kettle-handbook-11-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-06.png-blog.photo)
我们选择一个加密方式，使用第一个就可以了
![Kettle-handbook-11-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-07.png-blog.photo)
我们输入些基本信息然后next就可以
![Kettle-handbook-11-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-08.png-blog.photo)
![Kettle-handbook-11-09.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-09.png-blog.photo)
然后，我们得输入一段密钥
![Kettle-handbook-11-10.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-10.png-blog.photo)
好了，这里，就配置完成了
![Kettle-handbook-11-11.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-11.png-blog.photo)
![Kettle-handbook-11-12.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-12.png-blog.photo)

# 2. 用PGP加密文件
好了，这里，我们新建一个作业，我们主要使用这2个控件
![Kettle-handbook-11-13.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-13.png-blog.photo)
一个很简单的流程，
![Kettle-handbook-11-14.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-14.png-blog.photo)
我们做些简单的配置，
一个是GPG的目录（就是我们上面安装的那个）
![Kettle-handbook-11-15.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-15.png-blog.photo)
还有就是，我们的要加密的文件和一个目标文件名，注意，这里我们得填写一下“用户ID”，就是我们前面新建的那个用户名就可以了
![Kettle-handbook-11-16.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-16.png-blog.photo)
![Kettle-handbook-11-17.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-17.png-blog.photo)
这里，可以勾选一下，目标是一个文件
![Kettle-handbook-11-18.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-18.png-blog.photo)
好了，然后，我们执行下就可以了
我们源文件：
![Kettle-handbook-11-19.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-19.png-blog.photo)
加密后的文件：
![Kettle-handbook-11-20.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-20.png-blog.photo)
下面，我们再看看，怎样解密

# 3. 用PGP解密文件
知道了加密，解密也是一样的，
![Kettle-handbook-11-21.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-21.png-blog.photo)
这里的话，配置和上面差不多，这里，我们要填写一个“密钥”，就是我们上面创建时，输入的一个密码
![Kettle-handbook-11-22.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-22.png-blog.photo)
![Kettle-handbook-11-23.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-23.png-blog.photo)
我们运行一下，解密后，是一样的
![Kettle-handbook-11-24.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-11-24.png-blog.photo)
好了，就简单介绍到这里
