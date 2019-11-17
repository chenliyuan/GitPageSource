title: numpy函数总结
date: 2019-10-19 21:36:29
---
==DataFrame用法==
### 一、类型转换
 Numpy matrices必须是2维的,但是 numpy arrays (ndarrays) 可以是多维的（1D，2D，3D····ND）. Matrix是Array的一个小的分支，包含于Array。所以matrix 拥有array的所有特性。 
```
data = DataFrame(np.arange(16).reshape(4,4),index = list("ABCD"),columns=list('wxyz'))
a=data.as_matrix() #将dataframe形式的数据框data转化为矩阵a
 
  #将dataframe形式的数据框存储到Excel文件中：
data.to_excel(r'E:\pypractice\Yun\doc\2.xls', sheet_name='Sheet1')      


>>> from numpy import *
>>> a1 =[[1,2,3],[4,5,6]] #列表
>>> a1
[[1, 2, 3], [4, 5, 6]]
>>> a2 = array(a1)   #列表 -----> 数组
>>> a2
array([[1, 2, 3],
       [4, 5, 6]])
>>> a3 = np.mat(a1)      #列表 ----> 矩阵
>>> a3
matrix([[1, 2, 3],
        [4, 5, 6]])
>>> a4=a3[:,0:2]   #对矩阵的操作,选取其前两列的数据
>>> a4
matrix([[1, 2],
        [4, 5]])
>>> a5 = a3.tolist()   #矩阵 ---> 列表
>>> a5
[[1, 2, 3], [4, 5, 6]]
>>> a5 == a1
True
>>> a6 = list(a2)   #数组 ---> 列表
>>> a6
[array([1, 2, 3]), array([4, 5, 6])]
>>> a7=a2.tolist() #数组 ---> 列表,内部数组也转换成列表
>>> a7
[[1, 2, 3], [4, 5, 6]]
>>> a8 = array(a3)  #矩阵 ---> 数组
>>> a8
array([[1, 2, 3],
       [4, 5, 6]])
>>> a9 = mat(a2)   #数组 ---> 矩阵
>>> a9
matrix([[1, 2, 3],
        [4, 5, 6]])
>>> ################
>>> #在一维情况下的#矩阵 ---> 数组---> 矩阵结果不同
>>> #在一维情况下的列表 ----> 矩阵 ---> 列表结果不同
>>> a1 =[1,2,3,4,5,6] #列表
>>> a1
[1, 2, 3, 4, 5, 6]
>>> a3 = mat(a1)      #列表 ----> 矩阵
>>> a3
matrix([[1, 2, 3, 4, 5, 6]])
>>> a4 = a3.tolist()  #矩阵 ---> 列表
>>> a4
[[1, 2, 3, 4, 5, 6]]
>>> a4[0]
[1, 2, 3, 4, 5, 6]
>>> a5 = mat(array(a1))   #数组 ---> 矩阵
>>> a5
matrix([[1, 2, 3, 4, 5, 6]])
>>> a6 = array(a5)  #矩阵 ---> 数组
>>> a6
array([[1, 2, 3, 4, 5, 6]])
```
### 二、选择对象
1.选择特定列和行的数据
a['x'] 那么将会返回columns为x的列，注意这种方式一次只能返回一个列。a.x与a['x']意思一样。

取行数据，通过切片[]来选择
如：a[0:3] 则会返回前三行的数据。

2.loc是通过标签来选择数据
a.loc['one']则会默认表示选取行为'one'的行；

a.loc[:,['a','b'] ] 表示选取所有的行以及columns为a,b的列；

a.loc[['one','two'],['a','b']] 表示选取'one'和'two'这两行以及columns为a,b的列；

a.loc['one','a']与a.loc[['one'],['a']]作用是一样的，不过前者只显示对应的值，而后者会显示对应的行和列标签。

3.iloc则是直接通过位置来选择数据
这与通过标签选择类似
a.iloc[1:2,1:2] 则会显示第一行第一列的数据;(切片后面的值取不到)

a.iloc[1:2] 即后面表示列的值没有时，默认选取行位置为1的数据;

a.iloc[[0,2],[1,2]] 即可以自由选取行位置，和列位置对应的数据。

原文：https://blog.csdn.net/tanlangqie/article/details/78659988 

==转置==
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190129142722602.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190129142701422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
==numpy的array和python中自带的list之间相互转化==

```
a=([3.234,34,3.777,6.33])#a为python的list类型

np.array(a)#将a转化为numpy的array
array([  3.234,  34.   ,   3.777,   6.33 ])

a.tolist()#将a转化为python的list
```



==pandas中利用 .iloc 和 .loc 切片==

```
rows = pd.read_csv('test.csv')
row_slice = rows.iloc[0:2, 1:3]  # 取rows的0-1行，1-2列的元素
print(row_slice)
```
https://blog.csdn.net/qq1483661204/article/details/77587881

==numpy中reshape函数==
必须是矩阵格式或者数组格式，才能使用 .reshape(c, -1) 函数， 表示将此矩阵或者数组重组，以 c行d列的形式表示（-1的作用就在此，自动计算d(行或列)

```
a1=np.ones((3,2))
print(a1.reshape(-1,1))
结果：
[[1.]
 [1.]
 [1.]
 [1.]
 [1.]
 [1.]]
```

==Python取numpy数组的某几行某几列方法==
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181218180709674.png)

正确分析：

```
C_A = c[[0,2]]    #先取出想要的行数据
C_A = C_A[:,[2,3]] #再取出要求的列数据
print(C_A) #输出最终结果
1
2
3
```

原文：https://blog.csdn.net/qq_34734303/article/details/80631831
==stack==

```
np.vstack():在竖直方向上堆叠
np.hstack():在水平方向上平铺

import numpy as np
arr1=np.array([1,2,3])
arr2=np.array([4,5,6])
print (np.vstack((arr1,arr2)))
 #[[1 2 3]
 #[4 5 6]]
print (np.hstack((arr1,arr2)))
#  [1 2 3 4 5 6]

a1=np.array([[1,2],[3,4],[5,6]])
a2=np.array([[7,8],[9,10],[11,12]])
print (a1)
print (a2)
print (np.hstack((a1,a2)))
# [[ 1  2  7  8]
#  [ 3  4  9 10]
#  [ 5  6 11 12]]
```

原文 https://blog.csdn.net/m0_37393514/article/details/79538748
==np.random.choice==

参数意思分别 是从a 中以概率P，随机选择3个, p没有指定的时候相当于是一致的分布
```
a1 = np.random.choice(a=5, size=3, replace=False, p=None)
print(a1)
```
非一致的分布，会以多少的概率提出来

```
a2 = np.random.choice(a=5, size=3, replace=False, p=[0.2, 0.1, 0.3, 0.4, 0.0])
print(a2)
```
 replacement 代表的意思是抽样之后还放不放回去，如果是False的话，那么出来的三个数都不一样，如果是
True的话， 有可能会出现重复的，因为前面的抽的放回去了

--------------------- 

原文：https://blog.csdn.net/qfpkzheng/article/details/79061601 

==numpy添加新的维度：newaxis==

		newaxis放在第几个位置，就会在shape里面看到相应的位置增加了一个维数

![è¿™é‡Œå†™å›¾ç‰‡æè¿°](https://img-blog.csdnimg.cn/20181218180609497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Nub3d5YmVhcg==,size_16,color_FFFFFF,t_70)
来源：https://blog.csdn.net/xtingjie/article/details/72510834
==.dot()==
一开始看到这个函数的时候，总是把它想成点积，因为dot是点的意思。。。

后来在经历了dot()和*完全搞混，犯下错误之后，才终于弄明白了。

*dot()计算的是我们经常计算的矩阵乘法，设A(2 * 3), B(3 * 4), 那么dot(A, B)就表示两个矩阵相乘，得到的结果是一个2 * 4的矩阵。*

而*，相当于matlab中的.*, 也就是真正的点积，即元素对应相乘。

```
>>> a = [[1, 0], [0, 1]]
>>> b = [[4, 1], [2, 2]]
>>> np.dot(a, b)
array([[4, 1],
       [2, 2]])
```

两个向量a = [a1, a2,…, an]和b = [b1, b2,…, bn]的点积定义为：a·b=a1b1+a2b2+……+anbn

引用：https://blog.csdn.net/qq_25436597/article/details/78839059

==numpy.concatenate==


numpy提供了numpy.concatenate((a1,a2,...), axis=0)函数。能够一次完成多个数组的拼接。其中a1,a2,...是数组类型的参数

示例3：

```
>>> a=np.array([1,2,3])
>>> b=np.array([11,22,33])
>>> c=np.array([44,55,66])
>>> np.concatenate((a,b,c),axis=0)  # 默认情况下，axis=0可以不写
array([ 1,  2,  3, 11, 22, 33, 44, 55, 66]) #对于一维数组拼接，axis的值不影响最后的结果
#多维数组：
>>> a=np.array([[1,2,3],[4,5,6]])
>>> b=np.array([[11,21,31],[7,8,9]])
>>> np.concatenate((a,b),axis=0)
array([[ 1,  2,  3],
       [ 4,  5,  6],
       [11, 21, 31],
       [ 7,  8,  9]])

>>> np.concatenate((a,b),axis=1)  #axis=1表示对应行的数组进行拼接
array([[ 1,  2,  3, 11, 21, 31],
       [ 4,  5,  6,  7,  8,  9]])OC](这里写自定义目录标题)

```

#### 其他函数 np.arange np.repeat
```
import numpy as np
 
x = np.arange(5)  
print (x)
c=np.array(([1,2],[3,4]))
print (c)
print(np.repeat(c,2))
print(np.repeat(c,2,0))
print(np.repeat(c,2,1))

```
