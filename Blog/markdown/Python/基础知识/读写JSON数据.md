---
title: 读写JSON编码格式的数据
date: 2017-01-11 23:12:43
tags:
- Python
- 笔记
categories: 
- Python
---
#### 问题
读写JSON编码格式的数据
#### 解决方案
json 模块提供了一种很简单的方式来编码和解码Json数据，其中两个主要的函数json.dumps()和json.loads(),要比其他序列化函数库如pickle的接口少得多。示例：
```
# Dict => JSON_Str
import json 
data = {
    'name':'ACME',
    'shares':100,
    'price':542.23
}
json_str = json.dumps(data)
```
json编码字符串转换回一个Python数据结构：
```
data = json.loads(json_str)
```
如果你要处理的是问价而不是字符串，
```
# Writing JSON data
with open('data.json', 'w') as f:
    json.dump(data, f)

# Reading data back
with open('data.json', 'r') as f:
    data = json.load(f)
```
JSON 编码支持的基本数据结构类型为None bool int float 和 str 以及包含这些类型数据结构的lists tuples 和dictionaries. 对于dictionaries,keys 需要是字符串类型（字典中任何非字符串类型的key 在编码时会先转换成为字符串），为了遵循JSON规范 你应该只编码Python的list和dictionaries 而且，在web应用程序中，顶层对象被编码成一个字典是一个标准做法。
- JSON编码的格式对于Python的语法而已几乎是完全一样的，除了一些小的差异之外。比如。True会被映射为true， False被映射成false。而None 会被映射为null, 下面是一个例子，演示编码后的字符串效果; 
```
>>> json.dumps(False)
'false'
>>> d = {'a': True,
...     'b': 'Hello',
...     'c': None}
>>> json.dumps(d)
'{"b": "Hello", "c": null, "a": true}'
>>>
```

