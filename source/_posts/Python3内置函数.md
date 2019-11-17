title: Python3内置函数
date: 2019-07-18 19:25:14
---
### 1. 内建模块collections
**collections.Counter**计数器，计算列表中每项出现的次数，并返回**字典类型**，其中元素作为key，其计数作为value。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190326145102507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
当所访问的键不存在时，返回0，而不是KeyError；否则返回它的计数。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190526121803806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
### 2. sort 与 sorted 区别：

  - sort 是应用在 list 上的**方法**。  
   sorted 是**函数**，可以对所有可迭代的对象（比如字典）进行排序操作。

   **sorted 语法：**
  sorted(iterable, key=None, reverse=False)  
  如：

  ```
  array = [{"age":20,"name":"a"},{"age":25,"name":"b"},{"age":10,"name":"c"}]
  array = sorted(array,key=lambda x:-x["age"])
  #其中负号表示降序（默认升序），与reverse=True相同
  ```
  以上是使用关键词对元组进行的排序，lambda是隐函数固定格式；x标识列表中的一个元素，在这里标识一个元组，x可以任意起名。
 也可以对**set集合**进行排序，返回列表类型。
 ```
 >>>sorted({'mm','dd','aa'})
 >>>['aa', 'dd', 'mm']
 ```
 
 
 -  list 的 sort 方法返回的是对已经存在的列表进行操作；  
 而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

### 3. zip()和zip(*)
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。
zip 方法在 Python 2 和 Python 3 中的不同：在 Python 2.x zip() 返回的是一个列表。在3.x中返回的是一个对象，需要使用list将其转换为列表。
zip(*) 可理解为解压，返回二维矩阵式
```
>>>a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)     # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)  # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]
 
>>> a1, a2 = zip(*zip(a,b)) # 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式
>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
```
### 其他
- 数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。
- 列表中元素是元组，可以这样遍历
  ```
  list=[(1,3),(2,4)]
  for i ,j in list:
      print ("first:",i)
      print("second",j)
结果：
first: 1
second 3
first: 2
second 4
  ```