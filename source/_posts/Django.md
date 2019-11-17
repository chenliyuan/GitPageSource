title: Django
author: 躲不掉的风
date: 2019-11-10 15:41:27
tags:
---
- MVC的核心是思想是：解耦即减少相互联系，使得各个模块相互独立。
- Django : MTV 
- 安装：pip install Django==1.11.4  不写等号默认安装最新版本

  卸载：pip uninsall Django
- 验证 django.get_version()
- 创建项目：

  1. 创建目录  
  2. cd到该目录下，输入命令django-admin startproject projectname  
  3. tree . /F (win)查看目录层级
  
- 配置数据库：

      mysql数据库：安装pymysql
      1、setting配置：
      'ENGINE':'django.db.backends.mysql',
      'NAME':'Learn',
      'USER':'root',
      'PASSWORD':'rock1204',
      'HOST':'localhost',
      'PORT':'3306',
      2、在__init__.py中添加 import pymysql pymysql.install_as_MySQLdb()  不添加这些会报错

- 创建应用（app）：
可以创建多个应用；
  
   步骤：
   1. 在manage所在目录中，输入命令python manage.py startapp myapp
   2. 激活应用，在setting将myapp应用加入到app配置中
