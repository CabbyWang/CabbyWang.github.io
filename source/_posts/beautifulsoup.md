***
## BeatifulSoup
### 基本使用
````javascript
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
````

解析html
````python
from bs4 import BeautifulSoup
soup = BeutifulSoup(html, 'lxml')
soup.prettify()                 # 获取html文本(prettify有美化/装饰的意思，renturn一个标准的html格式，标准的缩进格式)
soup.head.prettify()
````
soup属性及方法
````python
soup.title                      # 获取<title>标签
# <title>The Dormouse's story</title>
soup.head
# <head><title>The Dormouse's story</title></head>
soup.title.string               # <title>标签内的字符
# "The Dormouse's story"
soup.title.name                 # 获取名称(通过标签获取标签名称)
# title

## 工厂方法
soup.new_string('',)
soup.new_tag(''[,href='',])
````

Tag(Name, Attributes)
````python
tag = soup.p
tag.string                   # 标签中的值
tag['class']                 # 获取p中的属性值(多值属性 返回类型为list)
# ['title']                     # 这里class为多值属性
tag.attrs                    # <p>标签属性字典
# {'class': ['title'], 'name': 'dromouse'}
tag.attrs['class']
# ['title']
tag['name']
# 'dromouse'
tag.attrs['name']
# 'dromouse'
tag.has_attr('class')       # 判断tag是否有是否包含'class'属性

# 重命名一个tag
tag.name = 'blockquote'     # 将<p>重命名为<blockquote>
# 改变属性的值
tag['class'] = 'verybold'   # 将class='title'变为class='verybold'
#添加或删除属性
del tag['class']            # 删除class属性
tag['id'] = 1
# 改变string属性
tag.string = 'New link text.'
# tag中添加内容
tag.append('Bar')           # <p>在这里添加内容</p>, 类似python中的append()方法
tag.insert(1, 'inserttxt')  # 把元素插入到指定位置，类似python中的insert()方法

tag.clear()                 # 移除tag中的内容(只移除tag中的内容)
ext_tag = tag.extract()               # 移除tag并作为方法结果返回，类似python中的pop()方法
ext_tag.extract()           # 被移除并返回的tag可以继续调用extract方法
tag.decompose()             # 将当前节点移除文档树并完全销毁
````

过滤器(find_all())
````python
soup.find_all('p')              # 返回一个list(<p>标签)
````

contents, children, descendants
````python
## .content返回一个列表
soup.contens                    # 将html以列表的方式输出
tag.contents                 # 将<p>的子节点以列表的方式输出
tag.contents[0]              # 获取第0索引
tag.contents[0].name         # 0索引值的标签名
len(tag.contents)

## .children返回一个生成器
for i, child in enumerate(tag.children):   # 对tag的子节点进行循环
    print(i, child)
#  0 <b>The Dormouse's story</b>

## .descendants(类似于os.walk的效果，遍历所有子节点)
for i, child in enumerate(tag.descendants):
    print(i, child)
# 0 <b>The Dormouse's story</b>
# 1 The Dormouse's story
````

get_text()
````
# 只得到tag中包含的文本内容
soup.get_text()
soup.p.text()
````
***
