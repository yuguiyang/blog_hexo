---
title: Python中文分词-jieba
date: 2017-08-19 10:00:00
categories:
- Python-基础
tags:
- Python
- 分词
---

刚刚在看Python的词云图，想要显示中文的时候，需要做一个分词，这里我们学习下jieba分词。

# 1. jieba中文分词
    jieba是Python中文分词的一个组件
    github地址：[https://github.com/fxsjy/jieba](https://github.com/fxsjy/jieba)

> 支持三种分词模式：精确模式，试图将句子最精确地切开，适合文本分析；全模式，把句子中所有的可以成词的词语都扫描出来, 速度非常快，但是不能解决歧义；搜索引擎模式，在精确模式的基础上，对长词再次切分，提高召回率，适合用于搜索引擎分词。支持繁体分词支持自定义词典MIT 授权协议

安装：
``` python
pip install jieba
```

官网上的小实例
``` python
import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print("Full Mode: " + "/ ".join(seg_list))  # 全模式

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("Default Mode: " + "/ ".join(seg_list))  # 精确模式

seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
print(", ".join(seg_list))

seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
print(", ".join(seg_list))
```

``` python
Building prefix dict from the default dictionary ...
Dumping model to file cache C:\Users\YUGUIY~1\AppData\Local\Temp\jieba.cache
Loading model cost 0.884 seconds.
Prefix dict has been built succesfully.
Full Mode: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学
Default Mode: 我/ 来到/ 北京/ 清华大学
他, 来到, 了, 网易, 杭研, 大厦
小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, ，, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造
```

到这里，想要的功能其实已经足够了，官网还有很多其他的介绍内容，很强大，后面需要的时候，再来补充，接着回去改那个词云的了先。