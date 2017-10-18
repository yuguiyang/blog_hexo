---
title: Python异常（1）- module 'urllib' has no attribute 'urlopen'
date: 2017-08-19 10:00:00
categories:
- Python-基础
tags:
- Python
- 异常
---

今天想复习下BeautifulSoup，就把之前的代码拿过来测试，发现报错了
![](http://upload-images.jianshu.io/upload_images/76024-e01a18d402a6796b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
import urllib
from bs4 import BeautifulSoup


#加载网址，获取当前页面
def getHTML(url) :
    page = urllib.urlopen(url)
    html = page.read()
    return html

html = getHTML('https://movie.douban.com/top250')
soup = BeautifulSoup(html, "html.parser")


for img in soup.find_all('img'):
    print(img.get('src'))
```

查了下，发下是Python3中，需要引入的模块变了
改一下就可以了
``` python
urllib.request.urlopen(url)
```