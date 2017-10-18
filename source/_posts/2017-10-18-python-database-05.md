---
title: Python-连接MySQL
date: 2017-1-18 10:00:00
categories:
- Python-数据库
tags:
- Python
- PyMySQL
---


MySQL是很常用的数据库，这里，我们来看看，怎样使用Python连接MySQL
连接MySQL，通常使用PyMySQL

#安装
PyMySQL主页：[https://pypi.python.org/pypi/PyMySQL](https://pypi.python.org/pypi/PyMySQL)
GitHub地址：[https://github.com/PyMySQL/PyMySQL/](https://github.com/PyMySQL/PyMySQL/)

通常我们使用
``` python
pip install PyMySQL

#或者加上本地源
pip install PyMySQL -i https://pypi.douban.com/simple/
```
就可以了，但是，我这里一个网速不行，一个报错，说编码有问题（错误忘记截图了）
那就试下，手动下载这个whl文件，进行安装
![](http://upload-images.jianshu.io/upload_images/76024-9ef03b5bb79d0269.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个安装没有问题
![](http://upload-images.jianshu.io/upload_images/76024-426f6fe18c376167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

然后，我们验证下
``` python
import pymysql

db = pymysql.connect("localhost","root","shishi","test" )

print(db)
```

![](http://upload-images.jianshu.io/upload_images/76024-eeb55c8d3c43f075.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

没有报错，说明成功了

#实例
使用的话，可以参考文档[http://pymysql.readthedocs.io/en/latest/modules/cursors.html](http://pymysql.readthedocs.io/en/latest/modules/cursors.html)

下面，我们看看简单的使用
``` python
# -*- coding: utf-8 -*-
"""
Created on Wed Oct 18 12:18:14 2017

@author: hexo
"""

import pymysql

#新建连接
con = pymysql.connect("localhost","root","shishi","test" )

#创建一个游标
cursor = con.cursor()
# 执行SQL
cursor.execute('show databases')
#获取所有结果集
rs = cursor.fetchall()

print(rs)

#关闭连接
con.close()
```

![](http://upload-images.jianshu.io/upload_images/76024-fc59b34fa600bc1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 中文乱码问题
暂时只想用来查询，所有其他的操作后续在整理，这里记录一个问题，中文乱码的问题
按照上面的代码来查询数据的话，如果有中文，会显示乱码

![](http://upload-images.jianshu.io/upload_images/76024-1de95349a7a66a08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们只要在新建连接的时候，修改下就行了
``` python
#新建连接，加上这个charset参数就行了
con = pymysql.connect("xxx","xxx","xxx","xxx",charset='utf8' )
```

![](http://upload-images.jianshu.io/upload_images/76024-ae873e8150b6c466.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## select
这里获取结果集的fetch方法

![](http://upload-images.jianshu.io/upload_images/76024-e336204f10fd0cbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)