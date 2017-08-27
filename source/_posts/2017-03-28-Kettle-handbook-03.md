---
title: Kettle手册（三）- 配置文件的使用及密码加密
date: 2017-03-28 22:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---
好了，我们上一回，练习了一个从数据库导出数据到Excel的例子，我们想一下，如果有很多个转换，我们没链接一次数据库，是不是都需要重复的输入那些数据库地址啊，数据库啊，用户名啊之类的。其实是不用的，我们可以使用变量的方式，写在配置文件中，下面，我们来看看。而且，我们平时开发，都有开发环境、UAT环境、生产环境，连接的地址都不一样，也不可能手动的去修改。

{% note primary %} 
## 1. Kettle的配置文件
{% endnote %}

配置文件在哪呢？Windows下，是再当前用户的目录下，一般再C盘，Users下面，有一个当前用户的文件夹，下面有.kettle文件夹
![Kettle-handbook-03-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-01.png-blog.photo)
进入之后，我们会看到一个kettle.properties的文件，我们的数据库配置信息，就可以放在这里，
![Kettle-handbook-03-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-02.png-blog.photo)

<!-- more -->

我们打开之后，编辑一下
![Kettle-handbook-03-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-03.png-blog.photo)
保存后，我们要重新启动下Kettle，因为这个配置文件是启动时加载的
重启后，我们将上一次，配置的转换打开，使用变量替换下之前的配置，Kettle中，我们使用${xxx}，表示引用一个变量，执行时，会自动替换
![Kettle-handbook-03-07.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-07.png-blog.photo)
我们测试下，同样时可以成功的。
![Kettle-handbook-03-08.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-08.png-blog.photo)
好了，这样，以后，不管是，数据库地址变化，还是部署生产，我们只需要修改配置文件就可以了。

{% note primary %} 
## 2. 密码加密
{% endnote %}

这里，顺便说下，加密的问题，比如，我们上面的数据库密码，是明文的，这样是不太安全的，而实际上，我们都是需要对密码进行加密的
我们进到Kettle的安装目录
![Kettle-handbook-03-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-04.png-blog.photo)
我们会看到，这里有一个Encr.bat，这就是可以加密的脚本
使用方法
![Kettle-handbook-03-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-06.png-blog.photo)
我们输入
``` bash
Encr.bat -kettle postgres
```
执行后，会生成，这样一个加密后的密码，然后，我们可以使用这个加密后的字符串，替换我们的密码
![Kettle-handbook-03-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-03-05.png-blog.photo)
``` bash
pg_password = Encrypted 2be98afc86aa7f2e4cb79ff228dc6fa8c
```
大家可以试下，这样也是可以的，好了，这个例子就到这。









