title: tensorflow学习实践一
author: 躲不掉的风
date: 2019-06-08 21:17:15
tags:
---
##### 查看已安装tf的版本和路径  
```
tf.__version__
tf.__path__
```

 第一课:从helloword开始  

![upload successful](/images/pasted-4.png)


![upload successful](/images/pasted-5.png)


![upload successful](/images/pasted-6.png)


![upload successful](/images/pasted-7.png)


![upload successful](/images/pasted-8.png)
##### numpy
用于科学计算，速度很快，因为本身主要用C语言实现，还有一些其他的优化。  
其他比较常用的库：IPython：Interactive python，新版本叫做jupyter
pandas\matplotlib
numpy：  

* ndarray：数组，相当于tensor，array是其缩写  
* ndim:维度，阶数 ,秩
* shape：各个维度的大小  
* size:所有元素总和  
* dtype:元素的类型，其中d是data的缩写

![upload successful](/images/pasted-9.png)  

什么是tensor(张量)？

![upload successful](/images/pasted-10.png)

![upload successful](/images/pasted-11.png)

![upload successful](/images/pasted-12.png)

【注】

```
>>> x=np.array([[1,3]])#双方括号是二维矩阵
>>> x.ndim
2
>>> y=np.array([1,2,3])#单方括号是一维向量
>>> y.ndim
1
```
![upload successful](/images/pasted-13.png)
四种tensor(注意大小写):  
* constant
* Variable
* Placeholder(占位符)
* SparseTensor
#### constant
必须参数：value
```
>>> import tensorflow as tf
>>> const=tf.constant(3)
>>> const #【注】tensor里面只有通过session才能输出
<tf.Tensor 'Const:0' shape=() dtype=int32>
>>> print(const)
Tensor("Const:0", shape=(), dtype=int32)
```

#### Variable
必填：初始值
```
>>> var =tf.Variable(3)
>>> var
<tf.Variable 'Variable:0' shape=() dtype=int32_ref>
>>> var1=tf.Variable(3,dtype=tf.int64)
>>> var1
<tf.Variable 'Variable_1:0' shape=() dtype=int64_ref>
```
#### Placeholder
必填项：dtype  其他：(shape,name)
先占住一个固定的位置，等着你之后往里面添加的一种
#### SparseTensor(稀疏张量)
平时用的比较少


```
>>> import tensorflow as tf
t>>> c=tf.constant([[2,3],[4,5]],name="const1",dtype=tf.int64)
>>> c
<tf.Tensor 'const1:0' shape=(2, 2) dtype=int64>
>>> sess=tf.Session()
>>> sess
<tensorflow.python.client.session.Session object at 0x11dedcd10>
>>> sess.run(c)#只有通过sess.run方法才会运行
array([[2, 3],
       [4, 5]])
>>> print sess.run(c)
[[2 3]
 [4 5]]
 
>>> if c.graph is tf.get_default_graph():#默认图不用创建
...     print "the graph of c is the default graph of the context"
... 
the graph of c is the default graph of the context
```

完整创建和关闭Session的两种方法：
```
#_*_coding:UTF-8 _*_
#引入tensorflow
import tensorflow as tf
#创建两个常量
const1=tf.constant([[2,2]])
const2=tf.constant([[4],[4]])

multiple=tf.matmul(const1,const2)
#创建Session会话对象并把操作执行的结果赋值给result
sess=tf.Session()
result=sess.run(multiple)
print (result)

if const1.graph is tf.get_default_graph():
        print("const1所在的图是上下文默认的图")
#关闭已用完的Session（会话）
sess.close()

#第二种方法来创建和关闭Session
with tf.Session() as sess:
        result2=sess.run(multiple)
        print("multiple运行结果是：%s" %result2)
```