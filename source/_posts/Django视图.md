title: Django视图View
author: 躲不掉的风
date: 2019-07-13 21:37:30
tags:
---
1. 视图快捷方式shortcut  
	- 常用的是render函数，该函数主要3个参数，第一个参数固定是request,第二个参数是模板文件名，第三个参数是字典类型，其中key就是
    ```
    {{...}}
    ```
    中所用的标识符，实际是双括号后返回的是key对应的值。如
```
#hello.html
	<h1>{{hello}}</h1>
#views.py
def hello(request):
     values={}
     values["hello"]="hello world!"
     return render(request,'hello.html',values)
```
	- render_to_string 返回字符串
    - redict（to,permanent=False,*args,**kwargs）第一个参数常常为模板
    - get_object_or_404 提取数据不存在则抛出异常
    - get_list_or_404使用filter方法提取数据不存在抛出异常
最终浏览器测试，显示hello world!  
2. HttpRequest对象即我们经常用的request  
有很多属性和方法，比如body\path\method\encoding\META...  
我们常用它的属性POST\GET，返回类似字典QueryDict的对象，包含表单中提交的数据。

  ```