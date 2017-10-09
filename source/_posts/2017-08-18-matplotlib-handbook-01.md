---
title: matplotlib手册(10)-用pyplot实现“房间里100个人玩游戏的例子”
date: 2017-08-18 08:00:00
categories:
- "数据可视化-Python&R"
tags:
- Python
- Matplotlib
---
之前有篇文章，说房间里有100个人，每人100块钱，的那个
原文介绍：[用数据分析告诉你这个世界很有意思](https://mp.weixin.qq.com/s?__biz=MjM5MDI1ODUyMA==&mid=2672939083&idx=2&sn=646721af2fb38c9d0ce85f32eb353c36&chksm=bce2f47c8b957d6ab60bc715b7d99c36491da71c4f24dd8509e943d061d416c69ced309a5179#rd)

![](http://upload-images.jianshu.io/upload_images/76024-476568183e907845.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

觉得挺有意思的，昨天发现pyplot也可以绘制动画，就来试试，主要是实现动画效果，其他的暂时先不考虑了
目前跑起来是可以，就是比较慢，还没找到原因
整体想法，就是
x轴表示玩家的序号，财富值用y轴来表示

<!-- more -->

#### 存款为0后，不在支付给别人，但可以收到别人给的钱
``` python
import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib.animation as animation  

#初始状态，共100人
total_people = 100  
#每个人的x轴坐标
x = np.arange(100)
#每人初始100块
people = [100]*total_people
#局数
game_round = 0
game_max_round = 17000

#初始绘图
fig,axes = plt.subplots()

axes.bar(x,people,facecolor='green') 
axes.set_title(u'Round: '+str(game_round))
axes.grid(True,axis='y')

#根据下标i,随机返回另一个下标
def give_to(i):
    i_to = i
    while i == i_to:
        i_to = np.random.randint(0,100)
    
    #print('from',i,'to',i_to)
    return i_to

#重新绘制图形
#1.当拥有的钱为0，则不再支出，但可以收入
def game(obj): 
    global people
    global game_round

    #还不知道咋让循环停止，就在这判断下
    if game_round < game_max_round:
        #清空当前轴
        plt.cla()
        
        #遍历100个人
        for i in range(total_people):
            #判断，当前人是否有钱
            if people[i] > 0 :
                #每个人拿出1块钱，给另一个人
                people[i] = people[i] - 1
                people_to = give_to(i)
                people[people_to] = people[people_to] + 1
                
                #print(people)
            else :
                pass
         
        
        game_round += 1
        #重新绘图
        axes.set_title(u'Round: '+str(game_round))
        axes.bar(x,sorted(people),facecolor='green')
        axes.grid(True,axis='y')
    else :
        pass


#循环调用游戏
ani = animation.FuncAnimation(fig, game, interval=0.1)  

plt.show()
```

![](http://upload-images.jianshu.io/upload_images/76024-68cdb43939666910.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不知道是不是代码有问题，最后有1个人那么高
代码跑的很慢，随机那块不知道有没有问题，等再研究下看看优化下

#### 可以贷款，即存款为负数
``` python
#2.允许借贷的情况，及拥有的钱可以为负
def game2(obj): 
    global people
    global game_round

    #还不知道咋让循环停止，就在这判断下
    if game_round < game_max_round:
        #清空当前轴
        plt.cla()
        
        #遍历100个人
        for i in range(total_people):
            #每个人拿出1块钱，给另一个人
            people[i] = people[i] - 1
            people_to = give_to(i)
            people[people_to] = people[people_to] + 1
         
        
        game_round += 1
        #重新绘图
        axes.set_title(u'Round: '+str(game_round))
        axes.bar(x,sorted(people),facecolor='green')
        axes.grid(True,axis='y')
    else :
        pass
```

![](http://upload-images.jianshu.io/upload_images/76024-a7cd5e345632acfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 35岁破产后，人生的走势
这里就以第6500轮游戏为基准，
``` python
# -*- coding: utf-8 -*-
"""
Created on Fri Aug 18 11:08:44 2017

@author: yuguiyang
"""
import numpy as np  
import matplotlib.pyplot as plt  
import matplotlib.animation as animation

#定义一个people类
class People:
    __data = 100
    __color = 'green'
    
    
    def __init__(self,data=100,color='green'):
        self.__data = data
        self.__color = color
    
    def __str__(self):
        return str(self.__data)+','+self.__color
    
    def set_data(self,data):
        self.__data = data
        
    def set_color(self,color):
        self.__color = color
    
    def get_data(self):
        return self.__data
    
    def get_color(self):
        return self.__color
    
    def give_money(self,money=1):
        self.__data = self.__data - money
        
    def rcv_money(self,money=1):
        self.__data = self.__data + money


#对数据进行排序后返回数据
def parse_people_data():
    global peoples
    
    people_data = []
    people_color = []
    for p in sorted(peoples, key=lambda p: p.get_data()):
        people_data.append(p.get_data())
        people_color.append(p.get_color())
        
    
    return people_data,people_color


####################################################

#初始状态，共100人
total_people = 100  
#每个人的x轴坐标
x = np.arange(total_people)

#每人初始100块,颜色是绿色
peoples = []
for i in range(total_people):
    peoples.append(People())
    

#局数
game_round = 0
game_max_round = 17000

#初始绘图
fig,axes = plt.subplots()

axes.bar(x,parse_people_data()[0],color=parse_people_data()[1]) 
axes.set_title(u'Round: '+str(game_round))
axes.grid(True,axis='y')


#根据下标i,随机返回另一个下标
def give_to(i):
    i_to = i
    while i == i_to:
        i_to = np.random.randint(0,100)
    
    return i_to

#重新绘制图形
#1.当拥有的钱为0，则不再支出，但可以收入
#参考plt_flash_demo2

#2.允许借贷的情况，及拥有的钱可以为负
def game2(obj): 
    global peoples
    global game_round

    #还不知道咋让循环停止，就在这判断下
    if game_round < game_max_round:
        #清空当前轴
        plt.cla()
        
        #遍历100个人
        for i in range(total_people):
            #每个人拿出1块钱，给另一个人
            peoples[i].give_money()
            people_to = give_to(i)
            peoples[people_to].rcv_money()
         
        #在第6500次游戏，修改负债者的颜色
        if game_round == 6500:
            for p in peoples :
                if p.get_data()<0:
                    p.set_color('red')
                else :
                    pass
            
        game_round += 1
        #重新绘图
        axes.set_title(u'Round: '+str(game_round))
        axes.bar(x,parse_people_data()[0],color=parse_people_data()[1])
        axes.grid(True,axis='y')
    else :
        pass
#循环调用游戏
ani = animation.FuncAnimation(fig, game2, interval=1)  

plt.show()  
``` 

6500忘记截图了，
![](http://upload-images.jianshu.io/upload_images/76024-c6b6a7234fa3a9b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/76024-3f97440bed645228.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

挺好玩儿的，还是有2个人逆袭的
这里，定义了一个People类来绑定财富值和颜色，感觉可以直接用pandas DataFrame就可以解决，一会儿找时间改下看看
后续再补充下其他情况