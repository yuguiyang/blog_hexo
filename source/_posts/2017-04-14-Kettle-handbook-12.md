---
title: Kettle手册（十二）- 控件使用-从步骤插入数据
date: 2017-04-14 11:24:44
categories:
- "ETL-Kettle"
tags:
- "Kettle"
---

这里介绍一个控件的小功能，也是最近才发现的，之前在“表输入”中要使用参数的话，一般都是使用变量，
其实，还有个功能也可以尝试使用
![Kettle-handbook-12-01.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-01.png-blog.photo)
整体流程就是这样，我们第一个 query_paramter，就是查询了我们想设置的参数
![Kettle-handbook-12-02.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-02.png-blog.photo)
然后，就是我们真正需要的，我们再表输入中，使用 “?”来占位，然后“从步骤插入数据”，选择上一个步骤，然后会将数据替换占位符
![Kettle-handbook-12-03.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-03.png-blog.photo)
最后，我们将文件导出即可，奥对了，我们可以改成日志控件，直接输出查看
![Kettle-handbook-12-04.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-04.png-blog.photo)
刚刚，上面还有一个“执行每一行”，这个就是，如果我们有多个参数，
![Kettle-handbook-12-05.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-05.png-blog.photo)
就可以使用这个参数了，很方便，好了，就介绍到这里先。
![Kettle-handbook-12-06.png](http://7xl61k.com1.z0.glb.clouddn.com/Kettle-handbook-12-06.png-blog.photo)