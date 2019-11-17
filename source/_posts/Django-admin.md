title: Django站点admin
author: 躲不掉的风
date: 2019-07-15 15:58:04
tags:
---
#### 站点配置:
1. 配置admin应用，默认已添加
2. 创建管理员用户
python manage.py createsuperuser
3. 汉化：LANGUAGE_CODE = 'zh-Hans'
TIME_ZONE = 'Asia/Shanghai'
4. 注册数据表（模型）admin.py

        from .models import  diffhsy,Runinfo
        #Register your models here.
        admin.site.register(diffhsy)
        admin.site.register(Runinfo)
      
5. 自定义管理页面

  1. 在admin.py文件中创建类（继承ModelAdmin）

  2. 在对应表注册后添加此类：admin.site.register(Batchinfo,biAdmin) #记得加，不然类没引用白写,示例：
```
      class biAdmin(admin.ModelAdmin):
          list_display = ['pk','createtime','other']

      admin.site.register(Batchinfo,biAdmin）
      admin.site.register(Diffhsy)
```
列表页属性：

    list_display=[]   设置显示的字段  
    list_filter=[] 过滤字段  
    search_field=[] 按照某字段搜索  
    list_per_page=int 每几个一页  
    添加/修改页属性  
    field=[]  修改属性先后顺序  
    fieldsets  给属性分组，与field不可同时使用  
    示例：

        fieldsets = [("condition", {"fields": ['servicename','rowkey','header_online']}), ("result",{"fields":['diff','run_no']})]
    
 在主表里添加数据时，同时也可添加对应的子表数据：

        class bitchTodf(admin.TabularInline):
            model = Diffhsy#子表
            extra = 2
        class biAdmin(admin.ModelAdmin):#主表
            inlines = [bitchTodf]

	布尔值显示难看？封装函数，然后列表显示时调用

        class BatchinfoAdmin(admin.ModelAdmin):
            def status(self):
                if not self.iserror:
                    return '错误'
                else:
                    return '正确'
            status.short_description = '状态'#设置页面列名
            list_display = ['pk','servicename','rowkey',status]#引用该函数

	执行动作位置问题（删除的那个控件）
    
        actions_on_bottom = True #执行动作改成下边
        actions_on_top = False

	使用装饰器完成注册（使用模型类）：

        @admin.register(Diffhsy)
        class DiffhsyAdmin(admin.ModelAdmin):


 #### 调试
 报错：[error] 13838#0: *1475 open() "/var/www/html/newworkbench/static/admin/css/responsive.css" failed   
 参考：https://blog.csdn.net/a657941877/article/details/8953233
 解决：将admin静态文件拷贝到nginx配置的static路径下。