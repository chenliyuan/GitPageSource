title: Django项目实例精讲
date: 2019-06-09 22:16:15
---
#### 认识Django
1. Python下有许多款不同的 Web 框架。Django是**重量级**选手中最有代表性的一位。许多成功的网站和APP都基于Django。

2. Django是一个**开放源代码**的Web应用框架，由**Python**写成。
3. Django采用了MVC的软件设计模式，即模型M，视图V和控制器C

#### 1. 创建隔离的python环境

推荐文章：https://www.cnblogs.com/jasmine-Jobs/p/7045016.html

```
  pip install virtualenv #安装
  virtualenv my_env#创建目录
  which python3  #找到python3目录
  virtualenv my_env -p /Library/Frameworks/Python.framework/Versions/3.7/bin/python3 
  #将python3作为解释器
  source my_env/bin/activate  #激活，开始使用
  deactivate 退出
```
 #### 2. 安装Django
 pip install Django==2.0.5
 #### 3. 创建项目
  Django-admin startproject mysite
    mysite/项目结构：
		manage.py
		mysite/
			  &nbsp;_&nbsp;__init__.py
			  &nbsp;_&nbsp;_setting.py
			 &nbsp;_ &nbsp;_url.py
			  &nbsp;_&nbsp;_wsgi.py
python manage.py migrate  #通过数据库迁移，初始状态下的应用程序表将在数据库中被创建。
创建完成后，根目录里应该多了个db.sqlite3

 #### 4.  运行开发服务器
	python manage.py runserver

	- 认识项目设置setting.py
	    DEBUG在生产环境下应该置为False.
	 - 项目和应用程序
	 “项目”我理解为所有应用程序基于某些设置项的安装结果。
	 “应用程序”（即app，也就是下文中的blog）是模型+试图+模板+url 的组合
	 应用程序与框架进行交互，提供特定的功能。
#### 5.  创建应用程序
 继续在虚拟环境里执行 python manage.py  startapp blog  
此时多了个blog/目录 和下面文件如下：
admin.py 可在该文件中注册模型  
app.py 包含了blog这个应用程序主要配置内容（之后在激活app时候用到）   
migrations包含了应用程序(app)的数据库迁移。迁移可以使Django跟踪模块 变化内容，并相应的同步数据库。  
model.py 包含了应用程序的数据模型，但是也可以被置空。  
test.py 用来测试的  
views.py包含逻辑内容  

#### 6. 数据模型
 每个模型是一个python类，为django.db.models.Model的子类，每个属性视为数据库的一个字段。  
 - 创建model.py文件中的数据表（即各python类）
 
```
from django.db import models
from django.utils import timezone
from  django.contrib.auth.models import User
# Create your models here.

class Post(models.Model):
    STATUS_CHOICE=(('draft','Draft'),('published','Published'))
    title=models.CharField(max_length=250)
    slug=models.SlugField(max_length=250,unique_for_date='publish')
    author=models.ForeignKey(User,on_delete=models.CASCADE,related_name='blog_posts',default="")
    body=models.TextField(default="")
    publish=models.DateTimeField(default=timezone.now)
    created=models.DateTimeField(auto_now_add=True)
    #auto_now_add=True：实例在第一次保存的时候自动保存当前时间，不能手动修改(但相对于auto_now，auto_now_add下次是可以的)。
    status=models.CharField(max_length=10,choices=STATUS_CHOICE,default='draft')
    class Meta:
        ordering=('-publish',)
    def __str__(self):
        return self.title
```

其中ForeignKey 的用法，待总结。。
 - 为了使Django跟踪应用程序并针对模型创建数据表，我们需要激活：
 &nbsp; &nbsp;编辑setting.py文件，像INSTALLED_APPS设置中加入app.py中所用到的类BlogConfig。格式为'blog.apps.BlogConfig'（若使用pycharm等IDE可能会给自动创建，可省略此步）

 - 迁移两步走
 创建了数据模型，我们最终需要的是定义**数据库**表，所以我们需要迁移：
 1）python manage.py makemigrations blog  #让 Django 知道我们在我们的模型有一些变更，此时生成了0001_initial.py文件
 
 			python manage.py sqlmigrate blog 0001 #命令可以查看某次迁移的SQL语句。
 2） python manage.py migrate  #将数据库与模型最终同步。

  #### 7. 站点管理
 -  ###### 登录
什么是站点？
对应的是之前说的admin.py应用。只要启动项目后，打开/admin/路径即可。
但是前提是需要创建超级用户（不然也登不进去呀）
python manage.py createsuperuser

 - ##### 向站点添加模型
重点来啦~~~ 怎么把模型添加到站点，以后方便我们维护呢？
编辑admin.py文件
```
from django.contrib import admin
from .models import Post.
admin.site.register(Post)  # 注册模型
```
之后打开站点就可以看到我们的模型啦，打开后就可以进行维护啦

 #### 8.  定制模型的显示方式
待整理
 #### 9. QuerySet和管理器
 -  ###### 创建对象
	方法一：两步走
```
 	post=Post(title="tile",slug="sss",body="post body",suthor=user)
 	#未插入到数据库
 	post.save()
 	#真正执行Insert操作
```
   方法二：一步到位
创建对象，并将其持久化至数据库中
 	
``
 	Post.objects.create(title="tile",slug="sss",body="post body",suthor=user)
``

 - ###### 更新对象
 	```
 	post.title="new title"
	post.save()
 	```
  - ###### 获取对象
  Django默认管理器objects。通过该管理器得到一个QuerySet对象,常用的方法有
Post.objects.all()
Post.objects.get(username='admin')
Post.objects.filter(publish__year=2017)
Post.objects.exclude(publish__year=2017)#排除
Post.objects.order_by(‘title’)默认升序，降序改为'-title'

  其他方法可参考官网：https://docs.djangoproject.com/en/2.0/ref/models
 - ######  删除对象
 post=Post.objects.get(username='admin')
post.delete()

 #### 10. 创建视图

```
from django.shortcuts import render,get_object_or_404
#get_object_or_404 若结果不存在则抛出404异常

from .models import Post

def blog_list(request):
    post_result=Post.objects.all()
    return  render(request,'post/list.html',{'post_value':post_result})
    #这里返回的字段中的Key即post_value用于在模板中引用
def blog_detail(request,year,month,day,slug):
    post_result=get_object_or_404(Post,slug=slug,publish__year=year,publish__month=month,publish__day=day)
    return  render(request,'post/detail.html',{'post_value':post_result})
```
#### 11. URL
这里分为两部分（当然也可以直接在项目url中一步到位，但是在有多个APP的情况下结构清晰，推荐两步）：
第一步：创建应用程序的url ：
blog/urls.py:
```
from django.urls import path
from . import  views

app_name='blog'  #本url的命名空间，被引用时使用

urlpatterns=[
    path('',views.blog_list,name='list_name'),
    path('<int:year>/<int:month>/<int:day>/<slug:slug>/',views.blog_detail,name='detail_name')
#路径中变量由上一级传参，动态
]
```
项目urls.py:

```
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('post/',include('blog.urls',namespace='blog'))#引用用应用程序，使用命名空间
]
```
为了获取url路径需要使用reverse方法，在models.py中添加此函数：

```
    def get_absolute_url(self):
        return reverse('blog:detail_name',args=[self.publish.year,self.publish.month,self.publish.day ,self.slug])
```
#### 12. 创建模板文件
一个基文件base.html和两个继承文件，目录：
base.html
post/
&nbsp;&nbsp;&nbsp;list.html
&nbsp;&nbsp;&nbsp;detail.html
base.html:

```
{% load static %}  # 加载静态文件
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
    <link href="{% static "css/blog.css" %}" rel="stylesheet">
</head>
<body>
<div id="content">
    {% block content %}
    {% endblock %}
</div>
<div id="sidebar">
    <h2>My base blog</h2>
    <p>this is my base blog;</p>
</div>
</body>
</html>
```
list.html：

```
{% extends "base.html" %}  #继承
{% block title %}My Blog{% endblock %}
{% block content %}
<h1>MY BLOG FOR LIST</h1>
    {% for p in post_value %}
    <h2>
    <a href="{{ p.get_absolute_url}}">  #使用此方法获取url
        {{ p.title}}</a>
    </h2>

    <p class="date">Published{{ p.publish }}by {{ p.author }}</p>
    {{ p.body|truncatewords:30|linebreaks }}
    {% endfor %}
{% endblock %}
```
detail.html:

 

```
{% extends "base.html" %}
{% block title %}{{post.title}}{% endblock %}
{% block content %}
    <h1>{{post_value.title}}</h1>
    <p id ="date">
    published {{ post_value.publish}}by{{ post_value.author }}
    </p>
    {{ post_value.body|linebreaks }}
{% endblock %}
```








## 【问题汇总】：
 - [1 ]  `File "manage.py", line 14
) from exc
^
SyntaxError: invalid syntax`

解决：使用virtualenv来做环境，更改pycharm中设置的编译器地址。

 - [ 2] `You are trying to add a non-nullable field 'author' to post without a default; we can't do that (the database needs something to populate existing rows)`
解决：在每个报错字段的后面加一个default=""即可

 - [ 3]` blog.Post.created: (fields.E160) The options auto_now, auto_now_add, and default are mutually exclusive. Only one of these options may be present`
 经过：因为后面追加上默认值default=timezone.now，提示auto_now_add=Ture与其不能共存，在网上一时也没找到更加合适的办法
 解决：第一次就只加默认值，指定两步迁移到数据库表；之后去掉default再次修改为auto_now_add=Ture，迁移成功。其中：
DateTimeField、DateField和TimeField三种类型可以用来创建日期字段，其值分别对应着datetime()、date()、time()三中对象。这三个field有着相同的参数auto_now和auto_now_add，

-[ 4]pycharm添加env python编译环境


setting-project中可添加和删除编译环境选项   
![upload successful](/images/pasted-19.png)
使用Pycharm给其配置编译器,提示pycharm配置虚拟环境    Envirement  location directory is not empty   
解决办法：将原来的代码里错误环境的目录找到，然后将venv删掉，再重新进来进行如下配置就没问题了
![upload successful](/images/pasted-20.png)

配置完成后，还是不对
![upload successful](/images/pasted-21.png)


![upload successful](/images/pasted-22.png)
换成下面的Existing environment成功
![upload successful](/images/pasted-23.png)
配置项目，编译器改成刚才Pycharm设置好的那个

![upload successful](/images/pasted-25.png)



![upload successful](/images/pasted-24.png)