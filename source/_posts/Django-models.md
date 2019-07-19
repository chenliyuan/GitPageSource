title: Django模型models
author: 躲不掉的风
date: 2019-07-12 17:45:16
tags:
---
1. 创建表
```
from django.db import models
class ContentTable(models.Model):
    title=models.CharField(default="",max_length=50)
    content=models.CharField(default="",max_length=1000)
```
2. 同步数据库(SQLLite3)
```
python manage.py makemigrations
python manage.py migrate
```
3. 使用
```
from wkbenchapp.models import ContentTable
ct = ContentTable.objects.create(title="标题", content="内容")
value = ContentTable.objects.filter(title="标题") #返回QuerySet object类型
for v in value:#其中每个记录v都是个ContentTable object
   print ("v",v.content)
```
  官方接口文档：https://docs.djangoproject.com/zh-hans/2.1/ref/models/querysets/#filter
  自强学院：https://code.ziqiangxuetang.com/django/django-models.html
  

4. QuerySet API主要分为这两类：  
1) Methods that return new QuerySets：  
  如：
  filter\all\...  
  可以使用List转换啊  

2) Methods that do not return QuerySets:  
  Returns the object matching the given lookup parameters  
  
  如：get\creater\count\...  
   - update  
   对指定的字段执行SQL更新查询，并返回匹配的行数。唯一的限制是它只能作用在表记录上，不能直接作用在模型object上，举例：
   ```
   >>> Entry.objects.update(blog__name='foo') # Won't work!
   >>>  Entry.objects.filter(blog__id=1).update(comments_on=True) # works!
   ```

  
5. 修改数据库操作实例
  
  ```
  	  ct=Content.objects.get(title=title)#获取对象
        ct.content=request.POST["autoTest"]#修改对应值
        ct.save()#保存
        ct=Content.objects.get(title=title)#重新查询后使用
  ```
6. **模型字段** 常用的模型字段有:  
 * BooleanFiled 字段值只包含True和Flase.如果没有设置Field.default属性，那么它的默认值是None.默认对应HTML的复选框：<input type="checkbox"...>   
 * CharFiled用于保存不太长的字符串。使用该字段必须要给出CharFiled.max_length,默认对应HTML的文本框<input type="text"...>
 - IntegerField 
 - TextField超长文本类型。
 * DateField对应Python中的datetime.date日期类型。其中可选参数DateField.auto_now，每当保存数据时都会将该字段值更新为当前时间。  
 DateField.auto_now_add只有当数据第一次创建的时候才保存当前时间。  
 auto_now、auto_now_add、default三个参数只能单独存在，且前两者字段值不可重写。  
 示例：
 ```
 date_field=models.DateField(default=datetime.datetime.now())
 ```
  - DateTimeFiled对应Python中的datetime.datetime时间类型 。auto_now、auto_now_add参数同DateField。   

 - DecimalField 指定小数位数的数值类型。
 - EmailFiled本质是CharFiled，不过会验证是不是有效的邮件地址。
 - FileField文件上传控件，不允许用primary_key属性。
7. ** 常用字段通用属性**
 - default
 - max_length
 - blank 当设置True时表单字段值允许为空。
 - choices 属性值为一个可迭代的对象，比如列表或者元祖。设置后网页中将会以下拉列表的形式显示。其中第一个值用于保存数据库中，第二个用于提高可读性。  
 - help_text
 - primary_key
 - unique
 - verbose_name
 
- **【报错】  **   
      django.db.utils.IntegrityError: UNIQUE constraint failed: wkbenchapp_content.id...  
	migrate更新表的时候不知道怎么回事总是报这个错，怎么改都不行，最后删除migrations里除了初始化外的所有日志，重新从makemigrations来一遍就顺利的好了。