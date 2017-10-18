---
title: Python基础（2）- 格式化format
date: 2017-08-01 08:00:00
categories:
- Python-基础
tags:
- Python
---


刚刚在练习pandas的时候，遇到一个格式化的问题，没有太理解，百度了下，这里整理下。
str.format()，是一个格式化字符串的函数，很强大
> str.format(*args, **kwargs)

主要是使用 {}和:
这里直接就复制过来了，我们可以通过参数的位置来输出
``` python
>>>"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
'hello world'
 
>>> "{0} {1}".format("hello", "world")  # 设置指定位置
'hello world'
 
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'
```

我们也可以通过key，value的形式来格式化
``` python
'name:{name},age:{age}'.format(name='lufei',age=20)
Out[46]: 'name:lufei,age:20'

p=['namei',20]

'name:{0[0]},age:{0[1]}'.format(p)
Out[51]: 'name:namei,age:20'
```

填充与对齐
![](http://upload-images.jianshu.io/upload_images/76024-cd49b83bff170fa1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
^、<、>分别是居中、左对齐、右对齐，后面带宽度
:号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充

#右对齐，长度为8，不够的默认用空格补全
'{:>8}'.format('189')
Out[52]: '     189'

#右对齐，长度为8，不够的用0补全
'{:0>8}'.format('189')
Out[53]: '00000189'

'{:a>8}'.format('189')
Out[54]: 'aaaaa189'

```

数值精度
这个说的挺全的，直接截图来吧
![](http://upload-images.jianshu.io/upload_images/76024-0040730729b50966.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考资料：
[Python format 格式化函数](http://www.runoob.com/python/att-string-format.html)
官方介绍：[Format String Syntax](https://docs.python.org/2/library/string.html#formatstrings)
[飘逸的python - 增强的格式化字符串format函数 ](http://blog.csdn.net/handsomekang/article/details/9183303)