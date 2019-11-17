title: Python--类成员
date: 2018-10-07 09:55:11
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
**私有属性是否可以被外部访问？ **

可以，但是需要使用_ClassName_methodName的形式如._Person__country(前面一条下划线，后面两条)

    class Person(object):
        __country = 'China' #私有属性
        def __init__(self,name,age):
            self.__query()
        def __query(self):
             print("country:%s" % self.__country)

    gf = Person('林小花',18)
    print(gf.__country)#报错，不能直接访问

    gf.__country = 'America' #非原来的私有属性，另外一个新字段了
    print(gf.__country)
    print(gf._Person__country)#原来字段没有变

    gf._Person__country = 'America2' #使用类名可以访问私有属性
    print(gf.__country)
    print(gf._Person__country)#被改变成America2
 **类方法和静态方法**
 
 类方法：将类本身作为对象进行操作的方法，第一个参数是cls，代表当前类对象，使用装饰器@classmethod。
 
 静态方法:是个独立的单纯的函数，不含有类或实例的任何属性和方法。
 
 **类变量和实例变量定义**

在类中声明的变量我们称之为类变量[静态成员变量]，

在init()函数中声明的变量并且绑定在实例上的变量我们称之为成员变量[实例变量]。

类变量直接可以通过类名来调用。
 
 ```
 class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
 ```
**类方法与成员方法**

类方法：使用类名来调用的方法

成员方法：使用对象来调用的方法

参考：https://www.jb51.net/article/159611.htm
 