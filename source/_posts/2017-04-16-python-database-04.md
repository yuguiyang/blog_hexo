---
title: SQLAlchemy手册（四）- 关联查询
date: 2017-04-16 10:00:00
categories:
- Python-数据库
tags:
- Python
- SQLAlchemy
---


上一回，我们介绍了，常用的单查询SQL，这里，我们介绍下，怎样进行关联查询，就是join的使用
数据库使用表信息：
t_class
![](http://upload-images.jianshu.io/upload_images/76024-ef7520ec11c905d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

t_student
![](http://upload-images.jianshu.io/upload_images/76024-2e40b06a15566531.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

t_student表存有t_class表的id，获取学生所属的班级信息
``` python
# -*- coding: utf-8 -*-

from sqlalchemy import MetaData, create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date, or_, and_
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship
from sqlalchemy import text, func
from sqlalchemy.orm import sessionmaker
import sys

reload(sys)
sys.setdefaultencoding('utf-8')

Base = declarative_base()
engine = create_engine('postgresql://postgres:shishi@localhost:5432/postgres')
Session = sessionmaker(bind=engine)

session = Session()

class StuClass(Base):
    __tablename__ = 't_class'


    id = Column(Integer,primary_key=True)
    class_name = Column(String(10))

    def __repr__(self):
        return "<StuClass(id='%s', name='%s')>" % (self.id, self.class_name)

class Student(Base):
    __tablename__ = 't_student'

    id = Column(Integer , primary_key=True)
    stu_name = Column(String(10))
    class_id = Column(Integer , ForeignKey('t_class.id'))

    stuClass = relationship("StuClass")

    def __repr__(self):
        return "<Student(id='%s', stu_name='%s', class_id='%s', class_name='%s')>" % (self.id, self.stu_name,self.class_id,self.stuClass.class_name)


#使用filter定义2张表的关系
for stu, stuc in session.query(Student, StuClass).filter(Student.class_id == StuClass.id).all():
    print stu,stuc

#使用join需要ForeignKey
for x in session.query(Student).join(StuClass).filter(StuClass.id == 803).all():
    print x
```

![](http://upload-images.jianshu.io/upload_images/76024-fc458339c2d78ba8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面，我们再写一个统计班级人数的SQL
![](http://upload-images.jianshu.io/upload_images/76024-8b65477439073d47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用SQLAlchemy来写的话，是这样的
``` python
for rs in session.query(StuClass.class_name,func.count('*').label('stu_count')).join(Student).group_by(StuClass.class_name).order_by(StuClass.class_name).all():
    print('班级:' + rs[0] + ' 人数：' + str(rs[1]))
```

![image.png](http://upload-images.jianshu.io/upload_images/76024-be088e8ba847918f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，同样的功能，我们换成子查询练习下
``` sql
 select a.class_name,b.num from t_class a
left join (
    select class_id , count(1) num from t_student group by class_id
)b on b.class_id = a.id
order by a.class_name
```

那SQLAlchemy是这样滴：
``` python
 sub = session.query(Student.class_id,func.count('*').label('num')).group_by(Student.class_id).subquery()

for sc,num in session.query(StuClass,sub.c.num).outerjoin(sub, StuClass.id == sub.c.class_id).order_by(StuClass.class_name).all():
    print sc,num
```

今天暂时先到这里，后面的话，官网上例子挺全的，大家可以自行练习下，后续也会再整理