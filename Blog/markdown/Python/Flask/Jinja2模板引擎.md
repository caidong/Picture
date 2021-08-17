---
title: Flask Jinjna2模板引擎
date: 2017-06-11 23:12:43

tags: 
- 笔记
- Flask
categories: 
- Flask
---
1. Jinja2能识别所有类型的变量，甚至是一些复杂类型，例入 列表 字典 和对象
```
<p>A value from a dictionary: {{ mydict['key'] }}.</p>
<p>A value from a list: {{ mylist[3] }}.</p>
<p>A value from a list, with a variable index: {{ mylist[myintvar] }}.</p>
<p>A value from an object's method: {{ myobj.somemethod() }}.</p>
```
2. Jinja2变量的过滤器

过滤器名 | 说明
---|---
safe | 渲染时不转义
capitalize | 把值的首字母转换成大写，其他字母转换成小写
lower | 把值转换成小写字母
upper|把值转换成大写形式
title|把值中每个单词的首写字母转换成大写
trim|把值的首尾空格去掉
striptags|渲染之前把值中所有的HTML标签都删掉


3. 控制结构
```
<!--if else-->
{% if user %}
    Hello, {{ user }}!
{% else %}
    Hello, Stranger!
{% endif %}


<!-- for -->
<ul>
    {% for comment in comments %}
        <li>{{ comment }}</li>
    {% endfor %}
</ul>

<!--宏支持 类似于Python中函数 -->
{% macro render_comment(comment) %}
    <li>{{ comment }}</li>
{% endmacro %}

<ul>
    {% for comment in comments %}
        {{ render_comment(comment) }}
    {% endfor %}
</ul>


<!--宏模板 保存在单独的文件中-->
{% import 'macros.html' as macros %}
<ul>
    {% for comment in comments %}
        {{ macros.render_comment(comment) }}
    {% endfor %}
</ul>

```
4. 模板继承
```
<html>
<head>
    {% block head %}
    <title>{% block title %}{% endblock %} - My Application</title>
    {% endblock %}
</head>
<body>
    {% block body %}
    {% endblock %}
</body>
</html>



{% extends "base.html" %}
{% block title %}Index{% endblock %}
{% block head %}
    {{ super() }}
    <style>
    </style>
{% endblock %}
{% block body %}
<h1>Hello, World!</h1>
{% endblock %}

```
5. 
