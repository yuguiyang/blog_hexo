---
title: SQLAlchemy手册（二）- 增删改查
date: 2017-04-13 10:00:00
categories:
- Python-数据库
tags:
- Python
- SQLAlchemy
---

好了，这里，我们来看看，怎么使用SQLAlchemy，先从最简单的增删改查来看看，这里我们使用PostgreSQL数据库

# 1. 连接PG数据库
![](http://upload-images.jianshu.io/upload_images/76024-6b93949a41c73de0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为SQLAlchemy底层连接，还是使用这些框架，所以我们想要连接PostgreSQL，还得先安装下这些，这里我们使用psycopg2
![](http://upload-images.jianshu.io/upload_images/76024-f06e472df6b056b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

不装的话，我们连接时，会报错
``` python
pip install psycopg2
```

![](http://upload-images.jianshu.io/upload_images/76024-40b4e6d434365f85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

安装好后，我们来看看怎样连接PG
我们再SQLAlchemy中连接数据库的话，需要使用这个叫做Engine的类，
关于介绍我们可以看官方的教程：[http://docs.sqlalchemy.org/en/rel_1_1/core/engines.html](http://docs.sqlalchemy.org/en/rel_1_1/core/engines.html)
![](http://upload-images.jianshu.io/upload_images/76024-af61c4fc6caa6fdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

他可以适配大部分数据库，大家可以自行尝试下
![](http://upload-images.jianshu.io/upload_images/76024-a83f56d963afee9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
#!/usr/bin/python

from sqlalchemy import create_engine

engine = create_engine('postgresql://postgres:shishi@172.16.201.12:5432/postgres',echo=True)

print engine
```

# 2. create table/drop table
这里，我们来看看怎样用orm的思想去create table、drop table
按照orm的思想，这里还有一个MetaData类得看下
![](http://upload-images.jianshu.io/upload_images/76024-7046c83fd79175e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的示例，我们定义一个table“t_users”,有2个字段，id，user_name
``` python
#!/usr/bin/python

from sqlalchemy import Table, Column, Integer, String, MetaData, create_engine

engine = create_engine('postgresql://postgres:shishi@172.16.201.12:5432/postgres',echo=True)

metadata = MetaData()

users = Table('t_users',metadata,
                Column('id',Integer),
                Column('user_name',String(20))
        )

#do the drop first
metadata.drop(engine)

#now create the table
metadata.create(engine)
```

我们执行下
上面，我们create_engine的时候，指定了echo=True，我们就可以看到输出语句了
这里的drop，有一个参数，checkfirst=True
,默认就是TRUE，会判断该表是否存在，如果存在就执行删除，create也是一样的，会先判断是否存在
![](http://upload-images.jianshu.io/upload_images/76024-f12075d0f342650a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看看数据库
![](http://upload-images.jianshu.io/upload_images/76024-488a5c91b77b5c1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

--晚上又看了看，感觉上面的方法可以使用，但是，貌似不是ORM思想的方法，又看了看官方的文档，自己先试试，明天再接着整理

# 3. 查询数据
好了，上面的Table方式，可以用来创建表，这里
参考官方的教程：[http://docs.sqlalchemy.org/en/rel_1_1/orm/tutorial.html](http://docs.sqlalchemy.org/en/rel_1_1/orm/tutorial.html)

## 3.1 定义Mapping类
和Java里的一样，我们得定一些实体类，和数据库中的表做映射
``` python
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class SomeClass(Base):
    __tablename__ = 'some_table'
    id = Column(Integer, primary_key=True)
    name =  Column(String(50))
```

像上面这样，我们就定义了一个类，我们来实际写一个看看
这里，我们映射了数据库中的“t_users”表，有2个字段，“id”和“user_name”
![](http://upload-images.jianshu.io/upload_images/76024-0968f5858c5022f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` python
#!/usr/bin/python

from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import create_engine
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class User(Base):
        __tablename__='t_users'

        id = Column(Integer,primary_key=True)
        user_name = Column(String(10))

        def __repr__(self):
                return "<User(id='%s', user_name='%s')>" % (self.id, self.user_name)
```

## 3.2 session
下面，我们就来查询下数据
``` python
#!/usr/bin/python

from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class User(Base):
        __tablename__='t_users'

        id = Column(Integer,primary_key=True)
        user_name = Column(String(10))

        def __repr__(self):
                return "<User(id='%s', user_name='%s')>" % (self.id, self.user_name)

engine = create_engine('postgresql://postgres:shishi@172.16.201.12:5432/postgres',echo=True)
Session = sessionmaker(bind=engine)
session = Session()

#query all users
user_list = session.query(User).all()
print user_list
```

上面的话，我们还用到了session，这里的session，暂时理解为，可以开一个事务，处理我们的逻辑，我们执行select的话，也需要这个
这里，我们就直接查出所有的记录就可以了

![图片.png](http://upload-images.jianshu.io/upload_images/76024-8d556afb02c09342.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 4. insert
``` python
new_user = User(id=666,user_name='ayang')
session.add(new_user)
session.commit()
```

![](http://upload-images.jianshu.io/upload_images/76024-9072ff2803b1d2f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 5.update
``` python
#update the user
update_user = session.query(User).filter_by(id=666).first()
print update_user

update_user.user_name='apple'
session.commit()
```

![](http://upload-images.jianshu.io/upload_images/76024-73e8a819df387063.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)