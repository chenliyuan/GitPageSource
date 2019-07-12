title: Django models
author: 躲不掉的风
date: 2019-07-12 17:45:16
tags:
---
1.创建表
```
from django.db import models
class ContentTable(models.Model):
    title=models.CharField(default="",max_length=50)
    content=models.CharField(default="",max_length=1000)
```
2.同步数据库(SQLLite3)
```
python manage.py makemigrations
python manage.py migrate
```
3.使用
```
from wkbenchapp.models import ContentTable
ct = ContentTable.objects.create(title="标题", content="内容")
ct.save()
value = ContentTable.objects.filter(title="标题") #返回QuerySet object类型
for v in value:#其中每个记录v都是个ContentTable object
   print ("v",v.content)
```
官方接口文档：https://docs.djangoproject.com/zh-hans/2.1/ref/models/querysets/#filter
自强学院：https://code.ziqiangxuetang.com/django/django-models.html

