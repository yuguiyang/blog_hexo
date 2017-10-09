---
title: Tableau实例-帕累托图
date: 2017-09-20 10:00:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---
Tableau实例
帕累托图

前面，我们了解了[《帕累托的故事》](https://yuguiyang.github.io/2017/09/18/jotting-01/) 和 [二八定律与长尾理论](https://yuguiyang.github.io//2017/09/18/analysis-method-01/)，这里，我们学习下，在Tableau中，如何适用Tableau来绘制帕累托图。


# 准备
数据源：官方数据源“示例-超市”
因为，帕累托分布，主要是20%的商品可以产生80%的价值，所以，我们可以使用示例数据中的订单数据，来看看订单的销量是否符合帕累托分布。

![数据源](http://upload-images.jianshu.io/upload_images/76024-b95435afb86c9e6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 初级-每个类别的销售额分布
我们先来研究下，看产品的类别销售额是否符合帕累托分布，帕累托图有一个柱形图，有一个折线图，
柱形图，表示每个类别的销售额，而折线图表示每个类别的销售额占比

柱形图就直接使用子类别和销售额就行了
![每个类别的销售额](http://upload-images.jianshu.io/upload_images/76024-d8b12c89ef4325c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后，我们实现折线图
在行中，再拖一个销售额

![](http://upload-images.jianshu.io/upload_images/76024-15ceac91c846b724.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<!-- more -->

然后修改标记的类型，改为“线”
![](http://upload-images.jianshu.io/upload_images/76024-c8c25bc1f1f3b1bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

效果图
![](http://upload-images.jianshu.io/upload_images/76024-483c6800fce90cea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但这个折线图，并不是我们要的，我们想要的是一个占总额的百分比
我们要修改第2个“总计销售额”，添加表计算
![](http://upload-images.jianshu.io/upload_images/76024-93bbe88f5b45395a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们主要计算类型，选择汇总-总计，并且勾选“添加辅助计算”，选择“总额百分比”
![](http://upload-images.jianshu.io/upload_images/76024-5f3203403d0caf28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们把修改好的“总额百分比”，拖到“标签”上，然后效果如图所示
![](http://upload-images.jianshu.io/upload_images/76024-b54c815aedd3bbf9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

顺手，我们修改下折线图的颜色，因为后面，我们还要合并呢
![](http://upload-images.jianshu.io/upload_images/76024-2efbd4852bac024a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们再折线图的y轴上，右键，单击选择“双轴”
![](http://upload-images.jianshu.io/upload_images/76024-75a6f4ffaf3f25a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

他自动会变成这样，我们得手动改成柱形图和折线图（颜色也白改了...）
![](http://upload-images.jianshu.io/upload_images/76024-6e96af2abe9baced.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

调整完之后，就是这样啦，
![](http://upload-images.jianshu.io/upload_images/76024-7caeefa153b36655.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们调整下柱形图的y轴刻度，让他更像些

![](http://upload-images.jianshu.io/upload_images/76024-f06a129d1a45c22a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/76024-47444732bcb67aa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/76024-2a98f1e8e1e85960.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 进阶-产品占比与销售额占比
上面，我们做的是，哪些类别的销售额占80%，这里，我们想根据产品来看，并且想知道产品的百分之多少占了80%的销售额

和上面的步骤类似，这里，我们使用产品名称，效果如图
![](http://upload-images.jianshu.io/upload_images/76024-60225105a25296c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是这里，产品实在是太多了，我们想让x轴更简洁一些，就显示百分比好了

未完待续......



