---
title: SQLAlchemy手册（一）-简介及安装
date: 2017-04-12 10:00:00
categories:
- Python-数据库
tags:
- Python
- SQLAlchemy
---

Python新手，目前学习中，最近发现个叫SQLAlchemy的ORM框架，就是类似Java里面的Hibernate啊，Mybatis啊之类的，这里也简单记录下。
官网地址：[http://docs.sqlalchemy.org/en/rel_1_1/](http://docs.sqlalchemy.org/en/rel_1_1/)

# 1. SQLAlchemy 介绍
这里直接摘一下百度百科的内容，简单说就是ORM框架，更加方便的去和数据库库连接。
![](http://upload-images.jianshu.io/upload_images/76024-e41d2dccabfa775d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2. SQLAlchemy安装
熟悉Python的同学应该很擅长这个了，而且官方文档上也有介绍，[http://docs.sqlalchemy.org/en/rel_1_1/intro.html#installation](http://docs.sqlalchemy.org/en/rel_1_1/intro.html#installation)

## 2.1 pip 安装
``` python
pip install SQLAlchemy
```

## 2.2 setup.py
``` python
python setup.py install
```

家里网速不太好，就直接下了个包，用这种方式安装了，没啥问题
![](http://upload-images.jianshu.io/upload_images/76024-72dc94f96ab47245.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们引入验证一下
![](http://upload-images.jianshu.io/upload_images/76024-ee4627473e05f0e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
好了，第一回，先简单说到这，下一回，我们来看看怎么使用