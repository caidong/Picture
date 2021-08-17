---
title: python 装饰器decorator
date: 2017-05-01 23:12:43
tags:
- 笔记
categories: 
- Python
---
**装饰器**: 是一种软件的设计模式，可以在不使用子类或改变源代码的情况下，动态的改变函数，方法，类的的功能。

- 装饰器demo
```
def makeblod(func):
    def wrap():
        return '<b>/ ' +func() +'</b>'
    return wrap

def makeitalic(func):
    def wrap():
        return '<i>'+func()+'</i>'
    return wrap

@makeitalic
@makeblod
def say():
    return 'hello'


print(say())

```

#### funtools 中wraps作用
Python装饰器（decorator）在实现的时候，被装饰后的函数其实已经是另外一个函数了（函数名等函数属性会发生改变），为了不影响，Python的functools包中提供了一个叫wraps的decorator来消除这样的副作用。写一个decorator的时候，最好在实现之前加上functools的wrap，它能保留原有函数的名称和docstring。
- 加wraps
```
# encoding: utf-8
from functools import wraps

def makeblod(func):
    @wraps(func)
    def wrap():
        """
        dec
        装饰粗体
        :return:
        """
        return '<b>/ ' +func() +'</b>'
    return wrap

def makeitalic(func):
    @wraps(func)
    def wrap():
        """
        装饰斜体
        :return:
        """
        return '<i>'+func()+'</i>'
    return wrap

@makeitalic
@makeblod
def say():
    """
    实际
    :return:
    """
    return 'hello'


print(say.__doc__,say.__name__)


('\n    \xe5\xae\x9e\xe9\x99\x85\n    :return:\n    ', 'say')

```

- 不加wraps
```
# encoding: utf-8
from functools import wraps

def makeblod(func):
    def wrap():
        """
        dec
        装饰粗体
        :return:
        """
        return '<b>/ ' +func() +'</b>'
    return wrap

def makeitalic(func):
    def wrap():
        """
        装饰斜体
        :return:
        """
        return '<i>'+func()+'</i>'
    return wrap

@makeitalic
@makeblod
def say():
    """
    实际
    :return:
    """
    return 'hello'


print(say.__doc__,say.__name__)



('\n        \xe8\xa3\x85\xe9\xa5\xb0\xe6\x96\x9c\xe4\xbd\x93\n        :return:\n        ', 'wrap')

```



#### 带参数的装饰器


```
def decorator_param(name):
    def decorator(func):
            @functool.wraps(func):
            def wrap(*args,**kwargs):
                #添加操作
                func(*args,**kwargs):
                return func(*args,**kwargs)
            return wrap
    return decorator
    
    
@decorator_param('name')
def usage_decorator():
    pass 
    
    
```


#### 解除一个装饰器

```
import functools

def hello(func):
    @functools.wraps(func)
    def wrap(*args,**kwargs):
        #装饰
        print('hello,')
        return func(*args,**kwargs)
    return wrap
    
@hello
def world():
    print('world')

#带装饰器版本

world()

#解除装饰器操作

orignal = world.__wrapped__
orignal()


```
#### 类实现装饰器形式
通过类的定义也可以实现装饰器形式
在类中通过使用__init__ 和__call__方法来实现
```
    class Test(object):
        # 通过初始化方法，将要被装饰的函数传进来并记录下来
        def __init__(self, func):
            self.__func = func
        # 重写 __call__ 方法来实现装饰内容
        def __call__(self, *args, **kwargs):
            print('wrapper context')
            self.__func(*args, **kwargs)


    # 实际通过类的魔法方法call来实现
    @Test  # --> show = Test(show) show由原来引用函数,装饰后变成引用Test装饰类的对象
    def show():
        pass


    show()  # 对象调用方法,实际上是调用魔法方法call,实现了装饰器
```


