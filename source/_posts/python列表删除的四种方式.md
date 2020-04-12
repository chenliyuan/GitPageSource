title: python列表操作
author: 躲不掉的风
date: 2020-04-11 21:06:51
tags:
---
##### 列表添加元素的三种方式
+号/append/extend/insert

```
one_list = ["Python"]
other_list=["C++","C#"]
other_tuple=("PHP","Ruby")
# one_list=one_list+other_list
# 1. 列表可使用+号直接相加['Python', 'C++', 'C#']
# one_list.append(other_list)
# one_list.append(other_tuple) 
#2. append对象可以作为整体['Python', ['C++', 'C#'], ('PHP', 'Ruby')]
# one_list.extend(other_list)
# one_list.extend(other_tuple)
#3. extend与append区别是不作为整体加入，拆分后融合['Python', 'C++', 'C#', 'PHP', 'Ruby']
# one_list.insert(0,other_list)
#4. 在某位置插入，作为整体与append一致[['C++', 'C#'], 'Python']
print(one_list)
```
http://c.biancheng.net/view/7118.html
##### 列表删除元素的四种方式
del/pop/remove/clear
```
lang = ["Python", "C++", "Java", "PHP", "Ruby","Java" ,"MATLAB"]
# del lang[2] 可使用切片删除连续元素
# lang.pop(2)  #五无索引默认删除最后一个
# lang.remove("Java") #根据元素值删除第一个符合条件的
# lang.clear() #删除列表所有元素，返回空列表
print(lang)
```

具体参考：http://c.biancheng.net/view/2209.html
