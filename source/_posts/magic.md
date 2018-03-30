---
title: python 魔术方法
date: 2018-03-29 13:25:48
tags: [python, magic method]
---

# Magic Methods

python中类似`__xxx__`的魔术方法, 感觉非常有意思, 这里把我遇到的部分魔术方法作个总结, 也方便自己理解和查阅, 之后会慢慢的完善- -

## `__class__`
之后再总结

***
## `__slots__`
我的初步理解, 限制对象可以用来获取和赋值的属性  
<br>直接上代码
```python
class A(object):
    pass

a = A()

>>> a.name = 'aa'     # 可以给a一个name属性然后赋值
>>> a.name            # 可以获取到name的值
'aa'
>>> a.age = '12'
'12'
>>> a.sex             # 但是不能获取a中没有的属性
# AttributeError: 'A' object has no attribute 'sex'
```
<!-- more -->


给B加上属性`__slots__`
```python
class B(object):
    __slots__ = {}

b = B()

>>> b.name = 'bb'       # 给b附加属性会报AttributeError
# AttributeError: 'B' object has no attribute 'name'
```

创建C给属性`__slot__`加上值
```python
class C(object):
    __slots__ = {'name', 'age'}        # __slot__ = ['name', 'age']  
    # 经测试这里set, list, tuple都可以
    def __init__(self, name, age):
        self.name = name
        self.age = age

c = C('wangsiyong', '21')

# # name和age在slots中, 可以获取属性值
>>> c.name
'wangsiyong'
>>> c.age
'21'
>>> c.sex = 'man'  # 给c附加slots中没有的属性, 会报AttributeError
# AttributeError: 'C' object has no attribute 'sex'
```

**总结:** 如果类中没有`__slots__`属性, 用户可以随意给对象添加属性,操作属性; 在类中定义`__slots__`后, 用户则只能对`__slots__`属性中的值进行操作了