title: Django路由URL
author: 躲不掉的风
date: 2019-07-14 09:23:11
tags:
---
![upload successful](/images/pasted-58.png)
- path函数的第一个参数是URL模式字符串，用于匹配URL  
第二个参数是用于处理URL请求的视图函数  
- 使用尖括号提取URL中的参数，如<int:year>  
- URL模式字符串不需要以/开头  
- 可使用类型转化器对参数类型进行转换，如：  
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

【问题】
1. 出师不利，刚开始使用可变参数就匹配不上   
Page not found (404)    
The current path, getpapertextss, didn't match any of these.  
最后查跟url没关系，错误信息误导了我，原来是因为Views中从数据库获取数据时为空报了404  
另外注意：  
1）正则匹配django2.0需要用re_path   
2）匹配的url后面有没有‘/’，与配置一致，除非设置中设置了默认加/