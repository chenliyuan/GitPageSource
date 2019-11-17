title: Django路由URL
author: 躲不掉的风
date: 2019-07-14 09:23:11
tags:
---
####  配置URL
- 在myapp中创建urls.py文件，然后建立连接，从project的url中include到myapp中的urls文件
    re_path(r'^',include('myapp.urls'))



- path函数的第一个参数是URL模式字符串，用于匹配URL  
第二个参数是用于处理URL请求的视图函数  
- 使用尖括号提取URL中的参数，如<int:year>  
- URL模式字符串不需要以/开头  
- url传参：在Django 2.0版本以后，这里不再使用正则，而是用
.../<参数类型:传入函数的形参名>/...如path(r'oldurl/<str:a>/<str:b>/', views.oldUrl)类型转化器对参数类型进行转换，如：  

      str：除了“/”以外的任意字符串，默认是它  
      int：匹配任意大于0的整数  
      slug:匹配任意slug字符串，可以包含任意ASCII字符、数字、-、_（汉字尝试没成功）

举例： 
```
urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<title>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
]
```
![upload successful](/images/pasted-58.png)
##### 【其他】

###### urls与path区别
   在使用Django的时候，多次遇到urls与path，不知道两者有什么区别。下面简单介绍一下两者

  在django>=2.0的版本，urls.py中的django.conf.urls已经被django.urls所取代。

  django.urls的用法：

  ```
  from django.urls import path
  from . import view

  urlpatterns = [
      path('', view.hello),
      path('world/', view.world)
  ]
  ```
  其中最大的改变如下： 
  
  - import urls被import path所取代

  - 如果是路径需要在路径的后面加上一个斜杠  /

  旧版本如下：

  ```
  from django.conf.urls import url

  from . import view

  urlpatterns = [
      url(r'^hello$', view.hello),
  ]
  ```
  新版本如下：

  ```
  from django.urls import path
  from . import view

  urlpatterns = [
      path('hello/', view.hello),
  ```
###### 可变参数就匹配不上 ？  
Page not found (404)    
The current path, getpapertextss, didn't match any of these.  
最后查跟url没关系，错误信息误导了我，原来是因为Views中从数据库获取数据时为空报了404  
另外注意：  
1）正则匹配django2.0需要用re_path   
2）匹配的url后面有没有‘/’，与配置一致，除非设置中设置了默认加/