title: Django项目实践一
---
- 本机之前一直装的事2.7，但是现在3.x已然成为趋势，准备更新3.7

   windows:
 
  下载python最新版本逐步进行安装（一定要选中pip）
     安装requests，直接pip install requests即可。
	Python 下载地址：https://www.python.org/downloads/
	
  Linux(centos 6.4):
 参考： https://www.cnblogs.com/dukuan/p/8252019.html
	wget https://www.python.org/ftp/python/3.4.4/Python-3.4.4.tgz

		ls
		tar xf Python-3.4.4.tgz 
		mkdir /usr/local/python3
		cd Python-3.4.4
		ls
		./configure --prefix=/usr/local/python3
		make all
		make install
 - 下载Django，并安装
 Django 下载地址：https://www.djangoproject.com/download/
 解压缩后安装：
  	D:\下载\Django-2.1.7\Django-2.1.7>python setup.py install
  	将这几个目录添加到系统环境变量中如： C:\Python33\Lib\site-packages\django;C:\Python33\Scripts。
```
>>> import django    #检查
>>> django.get_version()
```
 - 下载 安装pycharm专业版
	打开 http://idea.lanyus.com/
 - 创建项目，使用自己机器上的python编译器。添加app名称，IDE会默认创建app相关文件和配置。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019022014425410.png)
 - 启动服务
 在开发阶段，为了能够快速预览到开发的效果，django提供了一个纯python编写的轻量级web服务器，仅在开发阶段使用
可以在Terminal运行如下命令启动服务器：
python manage.py runserver（默认127.0.0.1:8000 也可以使用0.0.0.0:8888这样可以再其他机器访问此端口的这个服务）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190220145321590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
如果增加、修改、删除python文件，服务器会自动重启
如果想关闭服务器可以使用ctrl+c或点击run窗口的红色方块按钮
当然可以直接在目录上启动服务，以上是为了方便开发使用。
 - 视图调用模板简写
视图调用模板都要执行以上三部分，于是Django提供了一个函数render封装了以上代码
方法render包含3个参数
第一个参数为request对象
第二个参数为模板文件路径
第三个参数为字典，表示向模板中传递的上下文数据
打开booktst/views.py文件，调用render的代码如下

```
#coding:utf-8

from django.shortcuts import render

def index(request):
    return render(request,'booktest/index.html',{'title':'图书列表','list':range(10)})
```

 - django.urls path  拼写url正则表达式报错？
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
其中最大的改变如下：import urls被import path所取代

如果是路径需要在路径的后面加上一个斜杠  /

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

 - 提交表单 MultiValueDictKeyError 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190220182153159.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)原因：
找了好久原因，不是语法的错误，而是逻辑错误。不应该在matrix.html页面传参，matrix是接收参数跳转的页面。解决方法就是新起了一个search.html的页面，进行传参。

```
def getSchedule(request):
    context={}
    pid=request.GET['pid']
    sid=request.GET['sid']
    context["list"]=task(pid,sid)
    return render(request,"matrix.html",context)
```

 - 使用openpyxl
 https://www.cnblogs.com/anpengapple/p/6399304.html
eg:行添加,直接用sheet.append(rowlist)
 - AJAX的四种异步请求方式
 简单理解jQuery中$.getJSON、$.get、$.post、$.ajax用法
https://www.cnblogs.com/dongsh/p/3235487.html
 - 由于用 jQuery 实现 ajax 比较简单，所以我们用 jQuery库来实现
 -  js调试  console.log
 - Django 后台返回
 数组：return HttpResponse(json.dumps(a), content_type='application/json')
 字典：return HttpResponse(json.dumps(name_dict), content_type='application/json')
 - url前面加/，指的是根目录。不加则指的是当前页面的子路径。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190222121737705.png)
 - js清空标签内容：$("#result").html("")，而非$("#result").val("") 
val是value的缩写  是给value属性赋值而已  并不是清空节点内的html元素
但是你如果html('2')  就会变成<input type='text' value=''>2</input>   虽然这个写法不对  但是这个意思
- python小数处理
获取小数点小数部分

```
import math
a = 123.34
b = math.modf(a)#分开两部分
print(b)                 #(0.3400000000000034, 123.0)
print('%.2f' % b[0])     #保留两位小数0.34
```
- python3.5中，import sqlite3 出现 no module named _sqlite3的解决方法
检查自己有没有安装sqlite-devel(rpm -qa |grep sqlite-devel) ，没有的话yum -y install sqlite-devel
然后进入到Python目录，（cd python目录）
然后命令行输入./configure，然后make和make  install
这个时候可以输入python，进入python环境后，import sqlite3，看还会不会报错。
- ImportError: No module named openpyxl,xlrd
pip  install openpyxl
- ImportError: No module named 'requests'
pip install requests
- How to fix error: django.db.utils.NotSupportedError: URIs not supported
这个问题浪费了很长的时间解决，首先是尝试升级qlite的版本，但是升级后依旧不能解决，最后根据提示中的目录将base.py中的字段改后解决了。即参考文章中的第一个方法。摘自:https://blog.csdn.net/zhuangmezhuang/article/details/82776272
