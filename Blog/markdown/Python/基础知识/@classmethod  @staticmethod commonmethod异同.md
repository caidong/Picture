---
title: python 方法比较
date: 2017-08-16 23:12:43
tags:
- 笔记
categories: 
- Python
---

#### 方法比较及说明
- 示例
```python
class A(object):
    def foo(self, x):
        """
        普通方法
        :param x:
        :return:
        """
        print "executing foo(%s,%s)" % (self, x)

    @classmethod
    def class_foo(cls, x):
        """
        类方法
        :param x:
        :return:
        """
        print "executing class_foo(%s,%s)" % (cls, x)

    @staticmethod
    def static_foo(x):
        """
        静态方法
        :param x:
        :return:
        """
        print "executing static_foo(%s)" % x

a = A()


# 使用普通方法调用，第一个参数隐式传递给方法

a.foo(1)
# executing foo(<__main__.A object at 0xb7dbef0c>,1)

# 类调用普通方法
A.foo(1)
# 报错 unbound method foo() must be called with A instance as first argument (got int instance instead)


# 实例调用类方法使用
# 将隐式传递类本身给方法
a.class_foo(1)
# executing class_foo(<class '__main__.A'>,1)


# 类调用类方法
# 将方法定义为类方法，一般都是使用类直接调用
A.class_foo(1)


#静态方法 既不传递实例self也不传递类cls
#类调用静态方法
A.static_foo(1)
# executing static_foo(1)

# 实例调用静态方法
a.static_foo(1)
# executing static_foo(1)
静态方法将类的连接函数进行分组


```
2. @classmethod
    将方法转换成类方法，一个类方法接收 class 作为第一个参数，就像和实例方法接受实例一样。声明类方法，使用idiom:
```
class C:
    @classmethod
    def f(cls, arg1, arg2, ...): ...
```
@classmethod 是函数装饰器,他可以使用类调用和实例调用，如C.f() 和C().f(),除了类之外，该实例被忽略，如果派生类调用类方法，则派生类对象讲作为隐含的第一个参数传递

3. @staticmethod
    将方法转换成静态方法，
一个静态方法
