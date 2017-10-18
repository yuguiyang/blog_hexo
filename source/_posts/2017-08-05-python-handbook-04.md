---
title: Python基础（4）- collections
date: 2017-08-05 10:00:00
categories:
- Python-基础
tags:
- Python
---

昨天用到了这个collections模块，挺好用的，这里记录下。
官网介绍：[https://docs.python.org/3/library/collections.html](https://docs.python.org/3/library/collections.html)
博客：[廖雪峰的博客](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001411031239400f7181f65f33a4623bc42276a605debf6000)
这里介绍些好玩儿的例子。

# namedtuple
> collections.namedtuple(typename, field_names, *, verbose=False, rename=False, module=None)
Returns a new tuple subclass named typename. The new subclass is used to create tuple-like objects that have fields accessible by attribute lookup as well as being indexable and iterable. Instances of the subclass also have a helpful docstring (with typename and field_names) and a helpful __repr__() method which lists the tuple contents in a name=value format.


namedtuple是一个工厂函数，返回一个自定义的tuple类，可读性更强些。
通常我们使用tuple的时候，像这样
``` python
point_a = 1,3

point_b = 2,6

point_a
Out[37]: (1, 3)

point_b
Out[38]: (2, 6)

point_a[0]
Out[39]: 1

point_a[1]
Out[40]: 3
```

我们是那个namedtuple就可以这样了
``` python
from collections import namedtuple

Point = namedtuple('Point',['x','y'])

point_a = Point(2,2)

point_b = Point(3,3)

point_a
Out[45]: Point(x=2, y=2)

point_b
Out[46]: Point(x=3, y=3)

point_a.x
Out[47]: 2

point_b.y
Out[48]: 3
```

这样使用一个坐标位置，是不是可读性更强呢，而且用起来也很方便
我们可以看看这个Point是怎样定义的
``` python
print(point_a._source)
from builtins import property as _property, tuple as _tuple
from operator import itemgetter as _itemgetter
from collections import OrderedDict

class Point(tuple):
    'Point(x, y)'

    __slots__ = ()

    _fields = ('x', 'y')

    def __new__(_cls, x, y):
        'Create new instance of Point(x, y)'
        return _tuple.__new__(_cls, (x, y))

    @classmethod
    def _make(cls, iterable, new=tuple.__new__, len=len):
        'Make a new Point object from a sequence or iterable'
        result = new(cls, iterable)
        if len(result) != 2:
            raise TypeError('Expected 2 arguments, got %d' % len(result))
        return result

    def _replace(_self, **kwds):
        'Return a new Point object replacing specified fields with new values'
        result = _self._make(map(kwds.pop, ('x', 'y'), _self))
        if kwds:
            raise ValueError('Got unexpected field names: %r' % list(kwds))
        return result

    def __repr__(self):
        'Return a nicely formatted representation string'
        return self.__class__.__name__ + '(x=%r, y=%r)' % self

    def _asdict(self):
        'Return a new OrderedDict which maps field names to their values.'
        return OrderedDict(zip(self._fields, self))

    def __getnewargs__(self):
        'Return self as a plain tuple.  Used by copy and pickle.'
        return tuple(self)

    x = _property(_itemgetter(0), doc='Alias for field number 0')

    y = _property(_itemgetter(1), doc='Alias for field number 1')

```

下面还有个更好用的地方，我们再读取CSV或者数据库的时候，会返回结果集，这个时候用起来更方便，比如：
``` python
import csv
from collections import namedtuple
       
EmployeeRecord = namedtuple('EmployeeRecord', 'name, age, title, department, paygrade')
for emp in map(EmployeeRecord._make, csv.reader(open(r'D:\document\python_demo\employee_data.csv'))):
    print(emp.name, emp.title)
    print('emp:',emp)


runfile('D:/document/python_demo/demo_hi.py', wdir='D:/document/python_demo')
lufei leader
emp: EmployeeRecord(name='lufei', age='20', title='leader', department='onepiece', paygrade='100')
namei teacher
emp: EmployeeRecord(name='namei', age='19', title='teacher', department='onepiece', paygrade='999')

```

_make
``` python
somenamedtuple._make(iterable)
Class method that makes a new instance from an existing sequence or iterable.
```

# deque
我们使用list的时候，用下标查找很快，数据量大的时候，插入删除比较慢，deque是为了高效实现插入和删除的双向队列。
> deque:double-ended queue
>
>class collections.deque([iterable[, maxlen]])
Returns a new deque object initialized left-to-right (using append()) with data from iterable. If iterable is not specified, the new deque is empty.

``` python
from collections import deque

a = deque(list('abcdef'))

a
Out[80]: deque(['a', 'b', 'c', 'd', 'e', 'f'])

a.append('x')

a.append('y')

a
Out[83]: deque(['a', 'b', 'c', 'd', 'e', 'f', 'x', 'y'])

a.appendleft('w')

a
Out[85]: deque(['w', 'a', 'b', 'c', 'd', 'e', 'f', 'x', 'y'])

a.pop()
Out[86]: 'y'

a.popleft()
Out[87]: 'w'
```

这里扩展了很多方便的函数，appendleft(),popleft()等等

# defaultdict
可以设置默认值的dict，平时我们使用dict的时候，如果key不存在，会报错
> class collections.defaultdict([default_factory[, ...]])
Returns a new dictionary-like object. defaultdict is a subclass of the built-in dict class. It overrides one method and adds one writable instance variable. The remaining functionality is the same as for the dict class and is not documented here.

``` python
a = {'name':'lufe','age':20}

a
Out[105]: {'age': 20, 'name': 'lufe'}

a['name']
Out[106]: 'lufe'

a['age']
Out[107]: 20

a['score']
Traceback (most recent call last):

  File "<ipython-input-108-99f54e089332>", line 1, in <module>
    a['score']

KeyError: 'score'
```

我们使用defaultdict就可以避免这个错误
``` python
from collections import defaultdict

b = defaultdict(int)

b['name']='lufei'

b
Out[123]: defaultdict(int, {'name': 'lufei'})

b['age']
Out[124]: 0
```

这里我们设置默认是int型，默认值为0
``` python
x = defaultdict(0)
Traceback (most recent call last):

  File "<ipython-input-125-dd2052e23af0>", line 1, in <module>
    x = defaultdict(0)

TypeError: first argument must be callable or None


x = defaultdict(lambda : 100)

x
Out[127]: defaultdict(<function __main__.<lambda>>, {})

x['name']
Out[128]: 100
```

# Counter
是一个简单的计数器，
> class collections.Counter([iterable-or-mapping])
A Counter is a dict subclass for counting hashable objects. It is an unordered collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts. The Counter class is similar to bags or multisets in other languages.


``` python
from collections import Counter

cnt = Counter(['red', 'blue', 'red', 'green', 'blue', 'blue'])

cnt
Out[131]: Counter({'blue': 3, 'green': 1, 'red': 2})

cnt.most_common(1)
Out[132]: [('blue', 3)]

cnt.most_common(-1)
Out[133]: []

cnt.elements
Out[134]: <bound method Counter.elements of Counter({'blue': 3, 'red': 2, 'green': 1})>

cnt.most_common(3)[:-2:-1]
Out[137]: [('green', 1)]
```

这个most_common最好用了感觉，根据次数进行排名

当然，collections中还有很多其他的好用的类，我们可以参考官方文档。