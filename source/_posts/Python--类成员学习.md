
title: Python--类成员学习
---
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190218115154175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
#!/usr/bin/python
# -*- coding: UTF-8 -*-
class Foo():
    #静态字段
    country="公有静态字段"
    __name="私有静态字段"#带有__开头属于私有
    def __init__(self):
    #普通字段
        print (self.country)
        print ( self.__name)#类内部可以访问私有
    @staticmethod
    def static_func():
        """定义静态方法，无默认参数"""
        print ('这个是静态方法')
    @classmethod
    def  class_func(cls):
        print("这个是类方法")
    def ord_func(self):
        """定义普通方法，至少有一个self参数"""
        print("这个是普通方法")
    @property
    def price(self):
        """定义一个属性，有返回"""
        return "cheap"
class Foo_son(Foo):
    def showcountry(self):
        print (self.country)
    def showname(self):
        print (self.__name)
Foo.static_func()#调用静态方法
Foo.class_func()#调用类方法
f=Foo()
f.ord_func()#调用普通方法
f.price#调用属性
###################################################
Foo.country #类可以访问公有字段
f.country #对象可以访问公有字段
#f.__name #错误，私有字段，类无法访问，只能；类内部使用
####################################################
f1=Foo_son()#创建派生类对象
f1.showcountry()#派生类可以访问公有字段
#f1.showname()# 错误,派生类无法访问私有字段

```
