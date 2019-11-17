title: 语法_python3记录
author: 躲不掉的风
date: 2018-08-09 17:29:00
tags:
---
#### 矩阵乘法
分为两种方式

1.np.dot(A,B)
- 对于多维矩阵，计算真正意义上的矩阵乘积(数学线性运算)
- 对于一维矩阵，计算内积

2.np.multiply()或*

实现对应元素直接相乘，即mn和m*n相乘得到的还是m*n的矩阵

https://www.cnblogs.com/hezhiyao/p/8177832.html


    import numpy as np
    xx =np.array([[-1, 0, 1],[-2, 0, 2]])
    yy =np.array([[-1, 0, 1],[-2, 0, 2]])
    rt=xx*yy
    count=sum(rt) #每列相加
    print(rt)
    print(count)
    print(sum(count))
    
    返回值
    [[1 0 1]
    [4 0 4]]
    
    [5 0 5]
    10

#### 字符串判定

    isalnum()数字或字母
    isalpha()字母
    isdigit()数字
    isupper()大写
    istitle()首字母大写
    isspace()所有字母都是空白字符、\t、\n、\r
#### max函数
1)除了数值类型，字符串取字母表最靠后的字母

2）可以添加参数key，指定方法，入参即为比较的元素

    sample=[40,53,12,5,5,9,9]
    print(max(set(sample), key=sample.count))
#### yield和斐波那契数列
比如爬楼梯和生兔子问题都是斐波那契数列问题。我的理解比如爬楼梯要么从下两层爬上来，要么就是从下1层爬上来，一共两种方式，肯定就是这两种方法的总和，兔子问题需要推出来符合斐波那契数列数列规律。

	def fab(max):
    a,b,n=0,1,0
    while(n<max):
        yield b
        print("aaa")
        a,b=b,a+b
        n+=1
    f=fab(5)
    print(f.__next__())#1
    print(f.__next__())#aaa 1
    print(f.__next__())#aaa 2
 yield：返回值b；每次调用__next__()的时候执行到此关键字处。下次__next__()再继续执行yeild后面的语句。
 
 因为是迭代器，所以可以改成遍历获取：
 
     for i in fab(5):
        print (i)
    
 对于斐波那契数列还可以使用装饰器进行优化：
 
     def cache(fun):
        fibdict = {}
        print ("cache begin..")
        def wrapper(n):

            if n in fibdict:
                return fibdict[n]
            fibdict[n]=fun(n)
            return fibdict[n]
        return wrapper
    @cache
    def fib(n):
        return 1 if n <2 else fib(n-1)+fib(n-2)

    print(fib(7))
#### re.search和re.match
前者从整个字符串中寻找一个匹配，后者只匹配字符串的开始。

    import re
    a="worldhellodfddjhello"
    print(re.match("hello",a).group())#None
    print(re.search("hello",a).group())#hello

#### dict.fromkeys(seq,value)
或{}.fromkeys(seq,value)

取序列元素作为key，value作为默认值(无value就是None)

【注意】：这个方法因为是创建字典，所以会自动去重

    a = [1,5,5,7]
    print(dict.fromkeys(a,"defaultvalue"))
    
    #输出
    {1: 'defaultvalue', 5: 'defaultvalue', 7: 'defaultvalue'}
    
     list({}.fromkeys(plist).keys())
#### 文件读取

    read():读取整个文件，内容为字符串
    readline()：返回下一行，内容为字符串；
    readlines():返回list,保存有每行的数据。
####  下划线命名
python中主要存在四种命名方式：

1、object #无下划线：公用方法

2、_object #单下划线：半保护

      被看作是“protect”，意思是只有类对象和子类对象自己能访问到这些变量，
       在模块或类外不可以使用，不能用’from module import *’导入。
      __object 是为了避免与子类的方法名称冲突， 对于该标识符描述的方法，父
      类的方法不能轻易地被子类的方法覆盖，他们的名字实际上是_classname__methodname。
                  
3、__object  #双下划线：全私有，全保护

     私有成员“private”，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据，不能用’from module import *’导入。
     
4、_ _ object_ _     #前后都有双下划线：内建方法，用户不要这样定义

#### jsonpath模块
安装pip install jsonpath
```
from jsonpath import jsonpath

testdict={
    "config": {
        "host_huidu": "10.39.34.70:8080",
    },
    "name": "hotel",
    "groups": {
        "order" : [
            "/hotel/getHomepageOrderList",
            "/myelong/getHotelOrder",
            "/myelong/getHotellist"
        ]        
    }
}
ret=jsonpath(testdict,"$.name")
ret=jsonpath(testdict,"$.groups.order[1]")
ret=jsonpath(testdict,"$..order[1:3]")
print(ret)
```
#### 单例（Singleton）

单例是一种设计模式，应用该模式的类只会生成一个实例

在 Python 中，我们可以用多种方法来实现单例模式：

- 使用模块

- 使用 __new__

- 使用装饰器（decorator）

- 使用元类（metaclass）

重写new函数：
```
# 实例化一个单例
class Singleton():
    __instance = None
 
    def __new__(cls, age, name):
        #如果类属性__instance的值为None，
        #那么就创建一个对象，并且赋值为这个对象的引用，保证下次调用这个方法时
        #能够知道之前已经创建过对象了，这样就保证了只有1个对象
        if not cls.__instance:
            cls.__instance = super().__new__(cls)
        return cls.__instance
 
a = Singleton(18, "dongGe")
b = Singleton(8, "dongGe")
 
print(id(a))
print(id(b))
```
【注】 区别:cls是类本身的一个对象,self是类实例的一个对象

参考：https://segmentfault.com/a/1190000008141049

#### python 赋值 深拷贝	浅拷贝
    浅拷贝：即引用，共用一个地址，互相受影响。  
    深拷贝：完全独立的地址，互相不受影响。  
    赋值：字典，列表等对象赋值相当于浅拷贝，而int,str,float,bool在值改变后新增了内存地址存放新值，与原来不共用一个地址，相当于深拷贝。  

一般默认是浅拷贝，有速度快，占用内存小等特点
 
 #### yaml模块
  1. 安装：pip install pyyaml  

  **语法 **：  
   1. 大小写敏感 
   2. 使用缩进表示层级关系
   3. 缩进时不允许使用Tab，只允许使用空格
   4. 缩进的空格数目不重要，只要相同层级的元素左对齐即可
   5. ‘#’表示注释，从它开始到行尾都被忽略

      yaml模块常用的两个方法时yaml.load和yaml.dump

  2. yaml.load
   返回一个对象dict

      ```
      import yaml
      f = open(r'E:\AutomaticTest\Test_Framework\config\config.yml')
      y = yaml.load(f)
      print (y)
      ```
  3. yaml.dump 将一个python对象生成为yaml文档,除此还有safe_dump等  
      若有中文，必须添加**allow_unicode**参数   
      第二个参数，是要写进的文件
      
          dump(data, stream=None, Dumper=Dumper,
            default_style=None,
            default_flow_style=None,
            encoding='utf-8', # encoding=None (Python 3)
            explicit_start=None,
            explicit_end=None,
            version=None,
            tags=None,
            canonical=None,
            indent=None,
            width=None,
            allow_unicode=None,
            line_break=None)
          dump_all(data, stream=None, Dumper=Dumper, ...)  

          safe_dump(data, stream=None, ...)  

          safe_dump_all(data, stream=None, ...)  

举例：
      import yaml
      with open('abc.conf','w')as f:
         yaml.dump(self.config, f, default_flow_style=False,
         indent=2, allow_unicode=True)
      ```
      JSON转YAML
      ```
      import json,yaml
      str = '{ "foo": "bar" }'
      data = json.loads(str)
      yml = yaml.safe_dump(data,allow_unicode=True)
      print(yml)

  参考：http://www.361way.com/python-pyyaml-module/4271.html  
  参考：https://www.cnblogs.com/klb561/p/9326677.html

 #### json模块
  1. json.dumps[卸载掉对象]   

      把一个Python对象编码转换成--->json**字符串**
  2. json.loads[装上对象]    

		把json字符串解码转换成--->python**对象dict**

  	  其中 json 有下面三种样式：

      字典样式 '{"name":"gzj", "age":"23", "sex":"man"}'
      列表样式 '["gzj", 23, "man"]'
      字典和列表相互嵌套的样式 """["gzj", "{'age':'23'}"]""" 
      特别注意 JSON 字符串中的内容用双引号，而非单引号。

  3. json.dump和json.load均是针对文件的。
  json.dump
  我理解为两个动作，一个动作是将”obj“转换为JSON格式的字符串，还有一个动作是将字符串写入到文件中

	```
	a = {"name":"Tom", "age":23}
	with open("test.json", "w", encoding='utf-8') as f:
	  ... # Parameter sort_keys    是否按照字母排序
    ... # Parameter indent       缩进的空格数
    ... # Parameter separators   分割符号形式
	     json.dump(a,f,indent=4) 
	   # indent 超级好用，格式化保存字典，默认为None，小于0为零个空格
	   # f.write(json.dumps(a, indent=4))
	   # 和上面的效果一样！！！
	```
	json.load
	一个动作是将一个包含JSON格式数据从文件中取出来，还有一个动作是将其序列化为一个python对象dict
	
	**当打开文件的时候，注意加上encoding否则会因为中文报错！**

   import json
	with open("test.json", "r", encoding='utf-8') as f:
	    aa = json.loads(f.read())
	    f.seek(0)
	    bb = json.load(f)    # 与 json.loads(f.read())

#### encode 和decode
这里只说python3,不谈python2

	- 字符串通过编码转换为字节码，字节码通过解码转换为字符串   
 
   str（unicode）--->(encode)--->bytes，bytes--->(decode)--->str 

   python3默认编码是unicode编码，非utf8,需要经过encode才可能到utf8.
  
  ```
  name="小明"   #类型：<class 'str'>
  name1=name.encode("utf-8")  #加密输出：b'\xe5\xb0\x8f\xe6\x98\x8e'
  name2=name1.decode("utf-8") #解密还原：'小明'

  ```
   u'\u7b80' 是unicode, '\xe7\xae\x80'是byte , byte和unicode之间一一对应, 可以相互转换, 转换规则如下:
   ```
  >>>b'\xe7\xae\x80'.decode('utf-8')   #字节转unicode（str）
  '简'
  >>>u'\u7b80'.encode('utf-8')    #unicode转字节
  b'\xe7\xae\x80'
   ```
   
   参考：https://www.jianshu.com/p/4ced2bbc9334
   https://blog.csdn.net/qq_29053519/article/details/79170519

#### 装饰器和闭包

  自定义装饰器，常用作打日志和运行时间
  
      import time
      def logger(func):
          #如果原函数有参数，那闭包函数必须保持参数个数一致
          def mywrapper(x,y):#将函数外面包一层
              print(x,y)#添加的额外功能,打印日志(1,2)
              t1=time.time()
              func(x,y)#原函数add(a,b)
              t2=time.time()
              cost=t2-t1#额外功能：计算时间
              print("共计花费时间{}".format(cost))
          return mywrapper

      @logger
      def add(a,b):
          print("a+b={}".format(a+b))
          time.sleep(1)
      add(1,2) #相当于logger(add)
      
    
  装饰器属于闭包函数：什么是闭包函数？

##### 闭包函数
    
    1.函数内部定义的函数  
    2.包含对外部变量而非全局变量的引用，如：
    
    def outer():
    	x = 1
    	def inner():
          print("x=%s" %x)
          print("inner func excuted")
    	inner()#也可以return
      print("outer func excuted")

    outer()

    #####输出结果#########
    x=1
    inner func excuted
    outer func excuted
    
   闭包函数,它必须包含自己的函数以及一个外部变量才能真正称得上是一个闭包函数。如果没有一个外部变量与其绑定,那么這个函数不能算得上是闭包函数.

   有了以上基础,对于装饰器就好理解了. 

##### 装饰器

   外部函数传入被装饰函数名，内部函数返回装饰函数名。

      特点：  
         1.不修改被装饰函数的调用方式   
         2.不修改被装饰函数的源代码
        
    import time
    import random

    def timmer(func):
        def wrapper():
            start_time = time.time()
            func()
            stop_time=time.time()
            print('run time is %s' %(stop_time-start_time))
        return wrapper
    def auth(func):
        def deco():
            name=input('name: ')
            password=input('password: ')
            if name == 'egon' and password == '123':
                print('login successful')
                func() #wrapper()
            else:
                print('login err')
        return deco

    @auth   # index = auth(timmer(index))                 
    @timmer # index = timmer(index)
    def index():

      time.sleep(3)
      print('welecome to index page')
    index()
 
   如果有多个引用如上面的@auth,@timmer,顺序是从下至上。

   参考：https://www.cnblogs.com/huchong/p/7725564.html#_label1
   
   
 #### 格式化

	https://www.cnblogs.com/lvcm/p/8859225.html这篇文章总结的很好

 #### 线程
 python3常用的两个模块为：
 
 - _thread
 - threading(推荐使用)


线程中参数传递方法：

    1、仅使用元组传递初始化
        threading.Thread(target=song,args=(1,2,3)).start()
    2、仅使用字典传递 threading.Thread(target=方法名, kwargs={"参数名": 参数1, "参数名": 参数2, ...})

    threading.Thread(target=song,kwargs={"a":1,"c":3,"b":2}).start() #参数顺序可以变
    3、混合使用元组和字典 threading.Thread(target=方法名，args=（参数1, 参数2, ...）, kwargs={"参数名": 参数1,"参数名": 参数2, ...})

    threading.Thread(target=song,args=(1,),kwargs={"c":3,"b":2}).start()
   主要参数：
   
    target表示调用对象 , 即子进程要执行的任务  
    args表示调用对象的位置参数,元组类型。  
  方法介绍：    
  
    p.start() : 启动进程,并调用子进程中的p.run ()  
    p.is_alive () : 如果p仍然运行 , 返回True
threading参考：https://www.jb51.net/article/161728.htm

#### 处理小数  
参考：https://www.jb51.net/article/102248.htm
  - 向下取整 int()
  - 四舍五入 round()
   
     ```
  >>> round(3.25); round(4.85)
  3.0
  5.0
    ```
  - 向上取整 ceil()
  - 分别取整数部分和小数部分
  ```
  >>> import math
  >>> math.modf(3.25)
  (0.25, 3.0)
  >>> math.modf(4.2)
  (0.20000000000000018, 4.0)
  >>>b = math.modf(123.34)#分开两部分
  >>>print(b)                
  >>>print('%.2f' % b[0]) #保留两位小数0.34
  ```
#### string模块

        import string
        string.ascii_lowercase  #打印所有的小写字母
        string.ascii_uppercase  #打印所有的大写字母
        string.ascii_letters #打印所有的大小写字母
        string.digits  #打印0-9的数字
        string.punctuation  #打印所有的特殊字符

        string.hexdigits #打印十六进制的字符
        string.printable  #打印所有的大小写，数字，特殊字符
        
#### hashlib模块

	python的hashlib库的md5摘要是不可反解的，非常安全

      hash.update(arg)
      更新hash对象。连续的调用该方法相当于连续的追加更新。例如m.update(a); m.update(b)相当于m.update(a+b)。  
      hash.digest() 返回摘要，作为二进制数据字符串值,  
      hash.hexdigest() 返回摘要，作为十六进制数据字符串值,  
      hash.copy() 复制  
示例：加盐

 ```
  def my_md5(s:str,salt=None):
  s=str(s)
  if salt:
    s=s+salt
  m=hashlib.md5(s.encode())
  return m.hexdigest()
 ```
 或
 
 ```
 import hashlib
obj=hashlib.md5(b'12334')                #实例化md5的时候可以给传个参数，这叫加盐
obj.update("admin".encode("utf-8"))      #是再加密的时候传入自己的一块字节，
secret=obj.hexdigest()
print(secret)
 ```
#### 局部变量和全局变量
###### 列表 字典 集合 类 ----局部变量可以改全局变量，除了整数和字符串
  ```
  d = {}
  def test():
      d['url']='https://www.cnblogs.com/uncleyong/p/10530261.html'
  def test2():
      d['url']='https://www.cnblogs.com/uncleyong/'
  test()
  test2()
  print(d)
  #{'url': 'https://www.cnblogs.com/uncleyong/'}

  ```
但是如果重新创建则不是一回事

   ```
    info ={'age':18,  'url':'https://www.cnblogs.com/uncleyong/p/10530261.html'}
    def test():
        #重新创建了一个区别全局info的变量（地址变了，可用id函数验证）		
        info={}
        info['name'] = 'qzcsbj'
    test()
    print(info)
    #{'age': 18, 'url': 'https://www.cnblogs.com/uncleyong/p/10530261.html'}

 ```
###### global
  global需要在局部变量定义之前声明，否则报错
  
    res = None
    def calc(a,b):
        res = 0
        global res
        res = a+b
    calc(1,2)
    print(res)
    #报错:SyntaxError: name 'res' is assigned to before global declaration
　　
#### random模块
 random中的一些重要函数的用法：
 
      random.random()函数是这个模块中最常用的方法了，它会生成一个随机的浮点数，范围是在0.0~1.0之间。

      random.uniform()正好弥补了上面函数的不足，它可以设定浮点数的范围，一个是上限，一个是下限。

      random.randint()随机生一个整数int类型，可以指定这个整数的范围，同样有上限和下限值，如random.randint（0，4）包括0和4中的随机整数。

      random.choice()可以从任何序列，比如list列表中，选取一个随机的元素返回，可以用于字符串、列表、元组等。

      random.sample()可以从指定的序列中，随机的截取指定长度的片断，不作原地修改。