---
title: python高阶函数Filter
date: 2018-4-27 22:54:48
tags: [python, 高阶函数, filter]
---

# filter函数
filter(function, iterable)
filter函数是python中的高阶函数,  第一个参数是一个筛选函数, 第二个参数是一个可迭代对象, 返回的是一个生成器类型, 可以通过next获取值. 这里大致讲述下原理, filter()把传入的function依次作用于iterable的每个元素, 满足条件的返回true, 不满足条件的返回false, 通过true还是false决定将该元素丢弃还是保留.

下面是简单的操作, 直接上代码
- 处理list
```python
# 列表(list)
a = [1, 2, 3, 4, 5]
list(filter(lambda x: x > 3, a))
# [4, 5]
```

<!-- more -->

- 处理dict
```python
# 字典(dict)
d = {1: 'a', 2: 'b', 3: 'c', 4: 'd', 5: 'e'}
for i in filter(lambda x: x[0] > 3, d.items()):
    print(i)
# (4, 'd')
# (5, 'e')
```
这里使用`d.items()`, 所以返回的元素为元组, 下面代码可以做个对比
```python
d = {1: 'a', 2: 'b', 3: 'c', 4: 'd', 5: 'e'}
for i in filter(lambda x: x[0] > 3, d):
# 4
# 5
```
这里的`iterable`对象是字典本身`d`,  这里filter默认使用键值来筛选, 下面用法也可以进行一下, 方便理解
```python
d = {1: 'a', 2: 'b', 3: 'c', 4: 'd', 5: 'e'}
for i in d:
  print(i)
# 依次输出 1 2 3 4 5
```
这里直接遍历字典, 不使用`d.items()`, 默认遍历的是键值
```python
d = {1: 'a', 2: 'b', 3: 'c', 4: 'd', 5: 'e'}
2 in d
# True
'a' in d
# False
'a' in d.values()
# True
```
以上代码判断`2`是否存在字典`d`中, 直接使用`2 in d`, 默认在`keys`中去寻找, 如果要在`values`中去找, 则需要使用`'a' in d.values()`

好了- -, 这里一不小心扯得有点多, 跑题了......回归正题, 看代码
````python
a = [1, 2, 3, 4, 5]
filter(lambda x: x > 3, a)
# <filter at 0x25d4f2ae518>
f = _
list(f)
# [4, 5]
list(f)
# []
````
这里有点意思,  我在这里进行了两次`list(f)`的操作,  第一次可以输出正确的值, 第二次就是空值了. 试过好几次都是这样, 是不是很神奇...之前我看过一篇文章, 不过忘了是在哪看过的了, 有讨论过filter返回的生成器的问题, filter返回的是一个生成器, 开篇已经阐述过了, 但是这个生成器有点特殊, `只会存在一次`, 对, 只会存在一次, 本人感觉这是一种优化(内存优化?). 首先返回的是一个生成器就很优化了, 还加了一个只存在一次的设定, 感觉这很pythonic啊/哭笑, 但是这也是一个极难发现的坑- -!

继续折腾, 如果`filter`会有这种设定, 那`map`这种同样身为高阶函数的是不是也有相同的设定呢!!!
```python
a = [1, 2, 3, 4, 5, 6]
map(lambda x: x + 1, a)
# <map at 0x25d4f27a6a0>
m = _
list(m)
# [2, 3, 4, 5, 6, 7]
list(m)
# []
```
果然, 同为高阶的`map`也存在相同的设定

好了, 先折腾到这儿了- - 