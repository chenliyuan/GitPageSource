title: Django表单form
author: 躲不掉的风
date: 2019-07-15 15:04:43
tags:
---

问题：
'AddForm' object has no attribute 'cleaned_data'
由于么有调用is_valid，所以没有cleaned_data方法。
##### 步骤
- 新建一个 forms.py 文件

```
from django import forms
 
class AddForm(forms.Form):
    a = forms.IntegerField()
    b = forms.IntegerField()
 ```
 
 我们的视图函数 views.py 中
 
```
  #coding:utf-8
  from django.shortcuts import render
  from django.http import HttpResponse
  #引入我们创建的表单类
  from .forms import AddForm 
  def index(request):
      if request.method == 'POST':# 当提交表单时
          form = AddForm(request.POST) # form 包含提交的数据
          if form.is_valid():# 如果提交的数据合法
              a = form.cleaned_data['a']
              b = form.cleaned_data['b']
              return HttpResponse(str(int(a) + int(b)))

      else:# 当正常访问时建表单视图
          form = AddForm()
      return render(request, 'index.html', {'form': form})
    
```
对应的模板文件 index.html
```
<form method='post'>
{% csrf_token %}
{{ form }}
<input type="submit" value="提交">
</form>
```
表单域同model，有CharField、IntegerField等，具体见官方文档：
https://docs.djangoproject.com/en/2.2/ref/forms/fields/
参考：https://code.ziqiangxuetang.com/django/django-forms.html