---
title: Python基础（9）- 排序技巧
date: 2017-08-18 10:00:00
categories:
- Python-基础
tags:
- Python
---

通常，我们排序的时候，我们可以使用系统内置的sorted函数或list自带的sort函数。
参考文章：[http://www.jb51.net/article/57678.htm](http://www.jb51.net/article/57678.htm)
[https://docs.python.org/3/library/stdtypes.html?highlight=sort#list.sort](https://docs.python.org/3/library/stdtypes.html?highlight=sort#list.sort)

# 1. list.sort
> sort(*, key=None, reverse=False)This method sorts the list in place, using only < comparisons between items. Exceptions are not suppressed - if any comparison operations fail, the entire sort operation will fail (and the list will likely be left in a partially modified state).

sort是就地排序
``` python
list1 = [66, 48, 28, 69, 86, 96, 89, 37, 56, 33]

list1
Out[93]: [66, 48, 28, 69, 86, 96, 89, 37, 56, 33]

list1.sort()

list1
Out[95]: [28, 33, 37, 48, 56, 66, 69, 86, 89, 96]

#默认是升序，可以使用reverse降序
list1.sort(reverse=True)

list1
Out[97]: [96, 89, 86, 69, 66, 56, 48, 37, 33, 28]

```

key可以传入一个函数，会作用于每一个element，返回值会作为排序键
> key specifies a function of one argument that is used to extract a comparison key from each list element (for example, key=str.lower). The key corresponding to each item in the list is calculated once and then used for the entire sorting process. The default value of None means that list items are sorted directly without calculating a separate key value.

``` python
list2=list('AckjehLhct')

list2
Out[99]: ['A', 'c', 'k', 'j', 'e', 'h', 'L', 'h', 'c', 't']

list2.sort()

list2
Out[101]: ['A', 'L', 'c', 'c', 'e', 'h', 'h', 'j', 'k', 't']

list2.sort(key=str.lower)

list2
Out[103]: ['A', 'c', 'c', 'e', 'h', 'h', 'j', 'k', 'L', 't']
```

# 2. sorted
> sorted(iterable, *, key=None, reverse=False)Return a new sorted list from the items in iterable

sorted是返回一个排序后的新list，使用方式基本一样，只是
``` python
list3 = [84, 57, 15, 84, 40, 37, 66, 45, 11, 95]

list3
Out[107]: [84, 57, 15, 84, 40, 37, 66, 45, 11, 95]

sorted(list3)
Out[108]: [11, 15, 37, 40, 45, 57, 66, 84, 84, 95]

sorted(list3,reverse=True)
Out[109]: [95, 84, 84, 66, 57, 45, 40, 37, 15, 11]
```

sorted方法对所有的可迭代对象都有效
``` python
sorted({3:'a',1:'z',5:'b',2:'c'})
Out[110]: [1, 2, 3, 5]
```

使用sorted还能对复杂对象进行排序
``` python
l_op=[('namei',20,'a'),('lufei',19,'c'),('qiaoba',15,'b')]

l_op
Out[112]: [('namei', 20, 'a'), ('lufei', 19, 'c'), ('qiaoba', 15, 'b')]

#默认是按照每一个tuple的第一个元素来排序的
sorted(l_op)
Out[113]: [('lufei', 19, 'c'), ('namei', 20, 'a'), ('qiaoba', 15, 'b')]


sorted(l_op,key=lambda x: x[0])
Out[114]: [('lufei', 19, 'c'), ('namei', 20, 'a'), ('qiaoba', 15, 'b')]

#我们可以通过key函数，来指定要使用的排序键，
sorted(l_op,key=lambda x: x[1])
Out[115]: [('qiaoba', 15, 'b'), ('lufei', 19, 'c'), ('namei', 20, 'a')]

sorted(l_op,key=lambda x: x[2],reverse=True)
Out[116]: [('lufei', 19, 'c'), ('qiaoba', 15, 'b'), ('namei', 20, 'a')]

sorted(l_op,key=lambda x: x[2])
Out[117]: [('namei', 20, 'a'), ('qiaoba', 15, 'b'), ('lufei', 19, 'c')]
```

对于自定义的对象也是可以的
``` python
class People:
    
    def __init__(self,id,name):
        self.id=id
        self.name=name
        

p1 = People(10,'lufei')
p2 = People(5,'namei')
p3 = People(15,'qiaoba')

#指定我们要排序的字段就行了
for p in sorted([p1,p2,p3],key=lambda p: p.id):
    print(p.id,p.name)

#out:
5 namei
10 lufei
15 qiaoba

for p in sorted([p1,p2,p3],key=lambda p: p.name):
    print(p.id,p.name)

#out:
10 lufei
5 namei
15 qiaoba
```