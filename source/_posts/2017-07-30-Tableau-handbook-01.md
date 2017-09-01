---
title: 小白学习Tableau-标签云
date: 2017-07-30 18:57:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---
# 序

周末参加了2天的Tableau培训，发现这个东西做可视化分析还是很方便的，用户体验也很好。
![Tableau-handbook-01-01](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-01.png-blog.photo)

也听了很多大师的介绍，受益良多。不管怎样，要开始学些这个Tableau了。之前也有做过IBM Cognos的开发，2个产品对比一下，从直观上来看，就是Tableau的可视化效果好很多，而且还融合了一些分析功能，挺不错的。
很久之前就知道了Tableau其实，只是一直都没有去研究她。学习的一开始呢，先是模仿，熟练掌握了Tableau之后，就可以任意发挥了。Tableau入门的话，官方就有很多的资料，挺全的，还有视频教程。

<!-- more -->

# 标签云之作

这个比较简单，主要参考了jiyang大神的教程，后面有链接
我们使用Tableau自带的示例库

![Tableau-handbook-01-02](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-02.png-blog.photo)

我们主要就使用“类别”、“子类别”、“销售额”

## 我们把子类别拖到文本上

![Tableau-handbook-01-03](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-03.png-blog.photo)


这里会显示，所有的子类别，然后呢，我们希望子类别可以根据销售额的大小而控制大小

## 把销售额拖到标记-大小上
![Tableau-handbook-01-04](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-04.png-blog.photo)

默认的话，可能会显示这个树状图，但是我们并不想要这样，我们想要显示文本

我们修改下，我们把自动，修改为文本

![Tableau-handbook-01-05](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-05.png-blog.photo)
![Tableau-handbook-01-06](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-06.png-blog.photo)

好了，已经可以按销售额的大小来显示子类别的内容了
当然，我们一般见到的标签云，都是有颜色的，那我们就继续用销售额的大小来控制颜色

## 将销售额拖到颜色上
![Tableau-handbook-01-07](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-07.png-blog.photo)
![Tableau-handbook-01-08](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-01-08.png-blog.photo)

这样呢，我们就用销售额控制了文本的大小和颜色，

一个简单的标签云就实现了

好了，这个例子就是这样简单，小例子，发布在Tableau Public上，

[练习02-销售额标签云](https://public.tableau.com/profile/yuguiyang#!/vizhome/02-/sheet2)

# 附录（参考资料）

参考学习jiyang大神的作品：[https://public.tableau.com/profile/jiyang#!/vizhome/_3516/sheet0](https://public.tableau.com/profile/jiyang#!/vizhome/_3516/sheet0)