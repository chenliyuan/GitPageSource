title: Django项目实践二(偏代码实现)
date: 2019-07-12 20:14:56
---
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
-  views返回几种方式
    return render_to_response("search.html")
    属于shortcuts模块，常用需要2个参数，第一个是模板如hello.html，第二个是字典，其中字典key就是模板中的变量。
    return HttpResponse("success!")
    常用于直接返回字符串
  
  - 静态文件的使用
  settings.py 中两处注意：
  

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
 
*.html中引用（常加在link前）：

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