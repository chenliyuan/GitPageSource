title: Django模型models
author: 躲不掉的风
date: 2019-07-12 17:45:16
tags:
---
#### 定义模型model：

- 一个模型对应数据库一个表；继承model.Model类；
- 不需要定义主键，系统自动生成
- 外键关联：

      sgrade=models.ForeignKey("Grades") 
      #意思是Grades的主键作为Student sgrade的外键

- 数据库表生成两步走：
1. python manage.py makemigrations 创建迁移文件 
2. python manage.py migrate

- 测试数据操作：python manage.py shell
- 添加数据：本质是创建一个模型类的对象;示例:
	grade1=Grades()
	grade1.gname="python"
	grade1.gtime=datetime(year=2017,month=12,day=17)
	grade1.save()  #注意保存
- 查询支持切片,但不支持负数Grades.objects.all()[:10]
- 条件查询：grade1=类名.objects.get(pk=2)#主键查找用关键字pk
- 修改数据：grade1.gname="python22"   grade1.save()
- 删除数据：grade1.delete()
- 更新数据：update()  返回匹配的行数。唯一的限制是它只能作用在表记录上，不能直接作用在模型object上，返回执行行数。举例： 

       >>> Entry.objects.update(blog__name='foo') # Won't work!
       >>>  Entry.objects.filter(blog__id=1).update(comments_on=True) # works!
     若单个object更新，则跟添加数据方法一致。
- 如果只是检查 Entry 中是否有对象，应该用 Entry.objects.all().exists()
- 推荐用 Entry.objects.count()来查询数量
- list(es) 可以强行将 QuerySet 变成 列表
- 关联：
  1. grade1.student_set.all()  #对象名.关联类小写_set.all()
  #班级grades1里所有的学生
  2. grade1.student_set.create(...)
  #创建一条学生新数据 

- 模型类中定义元信息Meta :

  1.db_table定义数据表名，推荐小写，默认是项目表名_类名 
  2.ordering 排序 如ordering[-id] 加负号是倒序
- 自定义Manager类：

	模型管理器是Django的模型与数据库进行交互的接口，一个模型可以有多个模型管理器。object是manager类的一个对象；
    
 作用： 向管理器中添加额外的方法；修改管理器返回的原始查询集（重写get_queryset）



        class diffManager(models.Manager):
            def get_queryset(self):#重写原函数
                return super(diffManager,self).get_queryset().filter(isdelete=True)
            #super参数，第一个是子类类名，第二个一般为self
        class  Diffhsy(models.Model):
            dfhobj=diffManager() #调用自定义的Manager类，替代Django为模型默认生成的objects模型管理器
         #在view.py中调用自定义模型管理器
         Student.dfhobj.all()

- 创建对象

	作用:像数据库中添加数据
    
	方法：1. 模型中类中添加方法 2. 在自定义管理器里添加方法。
    
      class Batchinfo(models.Model):
          @classmethod
          def createbatch(cls,time,oth):
              bt=cls(createtime=time,other=oth)
          return bt
          
#### 模型查询：
    
  官方接口文档：https://docs.djangoproject.com/zh-hans/2.1/ref/models/querysets/#filter
  自强学院：https://code.ziqiangxuetang.com/django/django-models.html
  
 ##### 返回查询集(多个):

      filter(键=值，键=值)等价于 filter(键=值).filter(键=值)经过过滤器返回的仍然是查询集；
      可以有多个过滤器链式调用；
      exclude() 过滤掉符合条件的数据
      order_by()
      values()
    
##### 返回单个对象: 

      get()  1.若没找到符合条件的对象，会引发DoesNotExits异常2.若返回多条，会引发MultipleObjectsReturned异常
      count()  返回查询集对象的个数
      first()  返回查询集第一个对象
      last()   返回查询集最后一个对象
      exist()  若有数据返回True

    比较运算符:字段名__运算符关键字=值：
       isnull:Entry.objects.filter(pub_date__isnull=True)
       in:Entry.objects.filter(headline__in='abc')
       gt/gte/lt/lte
       date/year/month/day/week/week_day:如pub_date__date=datetime.date(2005, 1, 1)
    区分大小写，若前面加i则不区分：
      exact/iexact  : Entry.objects.get(id__exact=14)
      contains/icontains
      startswith/endswith/istartswith/iendswith
     pk：代表主键
     聚合函数：aggregate方法：
        Avg/Count/Max/Min:Student.object.aggregate(Max('sage'))
    关联查询：
        Grades.objects.filter(student__scontend__contains('李爱国'))
     F对象：使用模型的两个属性进行比较
        from django.db.models import F,Q
        Student.object.filter(num1__gt=F('num2')+20)#F对象也可以进行算术运算
     Q对象：
        Model.objects.filter(Q(x=1) | Q(y=2))
        Model.objects.filter(~Q(x=1)) #非，即取反


#### 示例

1. 创建模型
```
from django.db import models
class ContentTable(models.Model):
    title=models.CharField(default="",max_length=50)
    content=models.CharField(default="",max_length=1000)
```

3. views.py中使用
```
from wkbenchapp.models import ContentTable
ct = ContentTable.objects.create(title="标题", content="内容")
value = ContentTable.objects.filter(title="标题") #返回QuerySet object类型
for v in value:#其中每个记录v都是个ContentTable object
   print ("v",v.content)
```
 #### QuerySet API
  
 #### 常用字段类型


		·AutoField
			·一个根据实际ID自动增长的IntegerField，通常不指定如果不指定，一个主键字段将自动添加到模型中

		·CharField(max_length=字符长度)
			·字符串，默认的表单样式是 TextInput

		·TextField
			·大文本字段，一般超过4000使用，默认的表单控件是Textarea

		·IntegerField
			·整数

		·DecimalField(max_digits=None, decimal_places=None)
			·使用python的Decimal实例表示的十进制浮点数
			·参数说明
				·DecimalField.max_digits
					·位数总数
				·DecimalField.decimal_places
					·小数点后的数字位数

		·FloatField
			·用Python的float实例来表示的浮点数

		·BooleanField
			·true/false 字段，此字段的默认表单控制是CheckboxInput

		·NullBooleanField
			·支持null、true、false三种值

		·DateField([auto_now=False, auto_now_add=False]) --- 日期
			·使用Python的datetime.date实例表示的日期
			·参数说明
				·DateField.auto_now(最后更新时间)
					·每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为false
				·DateField.auto_now_add（创建时间）
					·当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为false
			·说明
				·该字段默认对应的表单控件是一个TextInput. 在管理员站点添加了一个JavaScript写的日历控件，和一个“Today"的快捷按钮，包含了一个额外的invalid_date错误消息键
			·注意
				·auto_now_add, auto_now, and default 这些设置是相互排斥的，他们之间的任何组合将会发生错误的结果

		·TimeField                                 ------时间
			·使用Python的datetime.time实例表示的时间，参数同DateField

		·DateTimeField               -------------------日期+时间
			·使用Python的datetime.datetime实例表示的日期和时间，参数同DateField

		·FileField
			·一个上传文件的字段

		·ImageField
			·继承了FileField的所有属性和方法，但对上传的对象进行校验，确保它是个有效的image

#### 字段选项
		·概述
			·通过字段选项，可以实现对字段的约束
			·在字段对象时通过关键字参数指定

		·null
			·如果为True，Django 将空值以NULL 存储到数据库中，默认值是 False

		·blank
			·如果为True，则该字段允许为空白，默认值是 False

		·注意
			·null是数据库范畴的概念，blank是表单验证证范畴的

		·db_column
			·字段的名称，如果未指定，则使用属性的名称

		·db_index
			·若值为 True, 则在表中会为此字段创建索引

		·default
			·默认值

		·primary_key
			·若为 True, 则该字段会成为模型的主键字段

		·unique
			·如果为 True, 这个字段在表中必须有唯一值

 
#### 错误调试 
  1.    django.db.utils.IntegrityError: UNIQUE constraint failed: wkbenchapp_content.id...  
	migrate更新表的时候不知道怎么回事总是报这个错，怎么改都不行，最后删除migrations里除了初始化外的所有日志，重新从makemigrations来一遍就顺利的好了。