title: Django后台admin
author: 躲不掉的风
date: 2019-07-15 15:58:04
tags:
---
步骤：
1. 添加model,如 model.py文件

```
# coding:utf-8
from django.db import models
 
 
class Article(models.Model):
    title = models.CharField(u'标题', max_length=256)
    content = models.TextField(u'内容')
 
    pub_date = models.DateTimeField(u'发表时间', auto_now_add=True, editable = True)
    update_time = models.DateTimeField(u'更新时间',auto_now=True, null=True)
```
其中第一位的汉字将作为添加时的项目名称。
2. 同步所有的数据表

```
# 进入包含有 manage.py 的文件夹
python manage.py makemigrations
python manage.py migrate
```
3. 修改 admin.py 

```
from django.contrib import admin
from .models import Article
admin.site.register(Article)#注册使用模型
```
4. 创建登录用户

```
python manage.py createsuperuser
 
# 按照提示输入用户名和对应的密码就好了邮箱可以留空，用户名和密码必填
 
# 修改 用户密码可以用：
python manage.py changepassword username
```
5. 访问 http://localhost:8000/admin/ 输入设定的帐号和密码, 就可以看到：

![upload successful](\images\pasted-61.png)

![upload successful](\images\pasted-63.png)
我们会发现所有的文章都是叫 Link object，这样肯定不好，比如我们要修改，如何知道要修改哪个呢？

我们修改一下 blog 中的models.py，添加
```
  def __str__(self):
     return self.title
        ```
 6. 报错：[error] 13838#0: *1475 open() "/var/www/html/newworkbench/static/admin/css/responsive.css" failed   
 参考：https://blog.csdn.net/a657941877/article/details/8953233
 解决：将admin静态文件拷贝到nginx配置的static路径下。