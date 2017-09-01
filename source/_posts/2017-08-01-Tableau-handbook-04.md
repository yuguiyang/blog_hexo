---
title: 小白学习Tableau-购物篮分析
date: 2017-08-01 10:57:00
categories:
- Tableau
tags:
- "数据可视化"
- Tableau
---
Tableau实例

> 关于购物篮的介绍可以参考[]()

# 目标
我们想要分析的是顾客在购买商品的时候，哪些商品会同时购买。

下面，我们直接开始开发Tableau

# 数据源
我们就是用Tableau默认自带的“示例-超市”

# 拖入两张订单表
因为我们需要分析的是每个顾客，在购买A商品的时候，还会购买哪些商品。只使用一张订单表，不是很容易看出来，所以我们需要拖入2张订单表，以此来更方便的处理数据。
因为我们要分析的是每个顾客，所以我们使用顾客ID去关联

![Tableau-handbook-04-01](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-04-01.png-blog.photo)

# 拖子类别进行分析
我们按照下图所示，进行拖拽
![Tableau-handbook-04-02](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-04-02.png-blog.photo)

# 剔除无效的值
手工剔除，好麻烦，如果数据是在数据库中，直接用SQL处理掉就可以了

![Tableau-handbook-04-03](http://7xl61k.com1.z0.glb.clouddn.com/Tableau-handbook-04-03.png-blog.photo)

# 参考文章
[人人都是可视化分析师系列：用Tableau玩转购物篮分析](http://tableau.analyticservice.net/%E4%BA%BA%E4%BA%BA%E9%83%BD%E6%98%AF%E5%8F%AF%E8%A7%86%E5%8C%96%E5%88%86%E6%9E%90%E5%B8%88%E7%B3%BB%E5%88%97%EF%BC%9A%E7%94%A8tableau%E7%8E%A9%E8%BD%AC%E8%B4%AD%E7%89%A9%E7%AF%AE%E5%88%86%E6%9E%90/)



