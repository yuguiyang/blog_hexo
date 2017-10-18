---
title: Python基础（1）- 私有变量
date: 2017-04-16 08:00:00
categories:
- Python-基础
tags:
- Python
---


今天简单看了看Python中的面向对象的一些教程，简单记录下，和Java中还是有很多类似的
看的是这个博客：[访问限制](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386820042500060e2921830a4adf94fb31bcea8d6f5c000)

比如我们定义一个Student类
一个__init__构造函数，初始化2个属性，一个名字，一个成绩；
还有一个打印函数，输出学生的名字和成绩
``` python
class Student(object):

    def __init__(self, name, score):
        "initial student"
        self.name = name
        self.score = score

    def print_score(self):
        "print student info"
        print '%s : %s' %(self.name, self.score)

s1 = Student('lufei' , 99)
s1.print_score()
```

了解Java的同学都知道，我们一般定义实体类的话，一般都是private，然后定义get、set方法，
如果只是上面的代码，我们就可以随便的调用name和score了
我们需要加上限制
``` python
def __init__(self, name, score):
    "initial student"
    self.__name = name
    self.__score = score
```

我们在变量前面加上“__"就可以了
![](http://upload-images.jianshu.io/upload_images/76024-ec3f050156f7071c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们要引用的话，就会报错了，同样的，我们也可以加上get、set方法来使用
``` python
def get_name(self):
        return self.__name

def set_name(self,name):
    self.__name=name

print s1.get_name()

s1.set_name('libai')
print s1.get_name()
```

![](http://upload-images.jianshu.io/upload_images/76024-e48108c2d2e25785.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

原文还有很多其他内容，大家可以自行看看，我就简单记录这些