title: Django问题记录
date: 2018-08-08 19:53:45
---
#### 环境准备
本机之前一直装的是2.7，但是现在3.x已然成为趋势，准备更新3.7

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
    
  #### 实践
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

- 模板前端与后台交互方法 $.getJSON

        $(document).ready(function () {
            $("#dl").click(function () {
                var sid=$("#sid").val();
                var pid=$("#pid").val();
            $.getJSON("/download/",{'sid':sid,'pid':pid},)
            });

            $("#submit").click(function () {
                var sd=$("#sid").val();
                var pd=$("#pid").val();
                $.getJSON("/getSchedule/",{'sd':sd,'pd':pd},function (ret) {
                    $("#result").html("");
                   for(var i =0;i<ret.length;i++){
                       $('#result').append(ret[i]).append("<br>")
                   }
                })
            });
        });
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
- 'WSGIRequest' object has no attribute 'urlopen'

-  views返回几种方式
    return render_to_response("search.html")
    属于shortcuts模块，常用需要2个参数，第一个是模板如hello.html，第二个是字典，其中字典key就是模板中的变量。
    return HttpResponse("success!")
    常用于直接返回字符串
  
- 静态文件的使用
  settings.py 中两处注意：
  
	1. setting文件  

  ```
      -INSTALLED_APPS中 注册了‘django.contrib.staticfiles’，默认生成的文件已注册。 
      -指定STATIC_URL 与 STATICFILES_DIRS 的值.

      INSTALLED_APPS=[
          '...',
          'django.contrib.staticfiles',
          '...',
      ]

      STATIC_URL='/static/'
      STATICFILES_DIRS=(BASE_DIR,'static')
  ```
  
  2.html文件中引用静态文件（常加在link前）：

  ```
  {% load staticfiles %}
  <link rel="stylesheet" type="text/css" href="{% static 'css/main.css' %}" />
  ...
  {% static '/image/test.png' %}
```
- 打开text文件乱码且每次打开的时候会多出一行？

  本地的ct.txt文件保存格式未必是utf-8，所以需要指定编码格式才更准确，使用newline防止在每次写入默认新添加一空行

  ```
  file=open("templates/ct.txt","w",encoding="utf-8", newline='')
  ```
- 表单提交后不调view接口？   
表单submit按钮类型写错，应该是submit类型，非button类型。