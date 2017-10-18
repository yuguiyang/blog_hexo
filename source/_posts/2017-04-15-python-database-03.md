---
title: SQLAlchemy手册（三）- 常用单查询
date: 2017-04-15 10:00:00
categories:
- Python-数据库
tags:
- Python
- SQLAlchemy
---

好了，在前面，我们大概了解了SQLAlchemy的使用，日常，我们可能经常会使用些复杂点儿的查询，我们先练习下

#1. common filter
这里，官方都有介绍，我们主要参考这里：[http://docs.sqlalchemy.org/en/rel_1_1/orm/tutorial.html](http://docs.sqlalchemy.org/en/rel_1_1/orm/tutorial.html)
里面讲的都很清楚，我们这里简单练习下
首先是基本的脚本，后面，我们直接写query
数据库表：
![](http://upload-images.jianshu.io/upload_images/76024-fe7972eaa875902a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
# -*- coding: utf-8 -*-

from sqlalchemy import MetaData, create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date
from sqlalchemy.orm import sessionmaker
import sys

reload(sys)
sys.setdefaultencoding('utf-8')

Base = declarative_base()

class Book(Base):
    __tablename__ = 't_book'


    id = Column(Integer,primary_key=True)
    name = Column(String(10),primary_key=True)
    publish_date = Column(Date,primary_key=True)

    def __repr__(self):
        return "<Book(id='%s', name='%s'" % (self.id, self.name)


engine = create_engine('postgresql://postgres:shishi@localhost:5432/postgres')
Session = sessionmaker(bind=engine)

session = Session()
```

我们一般的过滤的话，要使用filter这个函数
``` python
print '----：equals'
for book in session.query(Book).filter(Book.id==190):
    print book

print '----：not equals' 
for book in session.query(Book).filter(Book.name != 'Java'):
    print book

print '----：like'
for book in session.query(Book).filter(Book.name.like('白%')):
    print book
```

![](http://upload-images.jianshu.io/upload_images/76024-321f22e2757d7ce1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

常用的SQL中，我们还有and，or
我们需要引入其他类
from sqlalchemy import or_, and_print '----：or'for book in session.query(Book).filter(or_(Book.name.like('白%'),Book.id == 191)):print bookprint '----：and'for book in session.query(Book).filter(and_(Book.name.like('白%'),Book.id == 191)):print book

# 2. 自定义SQL
这里，我们使用text，来自定义自己的sql
``` python
from sqlalchemy import or_, and_

print '----：or'
for book in session.query(Book).filter(or_(Book.name.like('白%'),Book.id == 191)):
    print book  

print '----：and'
for book in session.query(Book).filter(and_(Book.name.like('白%'),Book.id == 191)):
    print book
```

![](http://upload-images.jianshu.io/upload_images/76024-e3fa39479ff43724.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在SQL中，我们经常会使用参数传递，这里也是可以的
``` python
print '-----: param'
for book in session.query(Book).filter(text("id=:p_id_1 or id=:p_id_2")).params(p_id_1=192,p_id_2=193).order_by(text("id asc")).all():
    print book  
```

到这里，自定义SQL貌似还不够灵活，不能像在数据库中随便写SQL那样，下面，我们再试试另一种方式
``` python
print '-----: custom'
for book in session.query(Book).\
        from_statement(text("select *from t_book where id = :p_id_1 or id = :p_id_2 ")).\
        params(p_id_1=190, p_id_2=191).all():
    print book
```

这下，我们就可以随便写我们的SQL了，

# 3.聚合函数
我们先看看count
``` python
from sqlalchemy import func

print '----: count'
print session.query(func.count('*')).select_from(Book).scalar()
print session.query(func.count(Book.id)).scalar()

print '----: group by'
for rs in session.query(func.count(Book.id) , Book.name).group_by(Book.name).all():
    print rs[1]+' count： '+str(rs[0])
```

![](http://upload-images.jianshu.io/upload_images/76024-da1b53e56723ddf8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)