title: python  OpenCV
author: 躲不掉的风
date: 2019-11-04 20:24:15
tags:
---
#### 安装
	pip install opencv-python
#### Demo   
    import cv2
    img=cv2.imread("D:/cly/xuanxiu/img/girl.jpg")
    imgGray=cv2.imread("D:/cly/xuanxiu/img/girl.jpg",0)#生成灰色图片
    print(img.shape)#图片高、宽、通道（即y轴，x轴，通道）
    print(img.size)#图总像素
    print(img.dtype)#图片数据类型是uint8

    print(imgGray.shape)#图片高、宽、通道；灰色图不返通道
    #操作：中值滤波
    result=cv2.medianBlur(imgGray,3)
    cv2.namedWindow('result',0)#创建窗口
    cv2.resizeWindow('result',300,400)#设置大小
    
    cv2.imshow("result",result)#显示图片  
    cv2.waitKey(0)#等待图片关闭
    cv2.destroyAllWindows()#销毁所有窗口
    
#####  cv2.resize()函数的使用
https://www.jb51.net/article/163516.htm

常用于等比缩小比例，若参数中不适用fx而是使用指定dsize的话，需要注意的是(宽，长)即横纵坐标的顺序，这里也与shape里出来的相反

      import cv2
      img = cv2.imread("./Pictures/python.png")
      print('Original Dimensions : ', img.shape)
      resized = cv2.resize(img, None, fx=0.6, fy=0.6, interpolation=cv2.INTER_AREA)
      print('Resized Dimensions : ',resized.shape)

#####  像素坐标
图片是由像素组成的矩阵，彩色图片访问方式为：

	img[i,j,c]

i表示图片的行数，j表示图片的列数，c表示图片的通道数（RGB三通道分别对应0，1，2）。坐标是从左上角开始。

灰度图片访问方式为：

	gray[i,j]  即  gray[row,col] 即gray[y,x]
 
 
 【注】坐标系这里有点晕，总结就是使用**像素**查找的时候前面是y后面是x跟正常是反着的(跟shape返回一致)，但是**坐标**点的话记得不要取错还是(x,y)
    若是直线坐标是[x,y,x1,y1]
 
 需要注意的是(容易弄反，记住下面图就好了)
 
     row == height == Point.y
     col == width  == Point.x


![upload successful](/images/pasted-97.png)

##### 图像二值化

好文章：https://www.cnblogs.com/ssyfj/p/9272615.html

就是把图像分成两种颜色：黑和白，1白色，0黑色

##### 噪声处理
https://blog.csdn.net/RRWJ__/article/details/101013012

    blur = cv.blur(img,(15,15)) #均值滤波
    blur = cv.GaussianBlur(img,(5,5),0)#高斯滤波
    blur = cv.medianBlur(img,5)#中值滤波，去除椒盐噪声
    blur = cv.bilateralFilter(img,9,75,75)#双边滤波
    
OpenCV提供了这种技术的四种变体。参考：https://blog.csdn.net/xiao_lxl/article/details/95346589

    cv2.fastNlMeansDenoising（） - 使用单个灰度图像
    cv2.fastNlMeansDenoisingColored（） - 使用彩色图像。
    cv2.fastNlMeansDenoisingMulti（） - 用于在短时间内捕获的图像序列（灰度图像）
    cv2.fastNlMeansDenoisingColoredMulti（） - 与上面相同，但用于彩色图像。
   
- 中值滤波：medianBlur 

  优点：中值滤波在一定的条件下可以克服常见线性滤波器如方框滤波器、均值滤波等带来的图像细节模糊，而且对滤除脉冲干扰及图像扫描噪声非常有效，也常用于保护边缘信息, 保存边缘的特性使它在不希望出现边缘模糊的场合也很有用，是非常经典的平滑噪声处理方法。

  缺点：但是中值滤波的缺点也很明显，因为要进行排序操作，所以处理的时间长，是均值滤波的5倍以上。

  python代码实现：
  https://blog.csdn.net/A_Z666666/article/details/81324288

- 双边滤波 bilateralFilter

  是一种非线性滤波方法。能够保持边界清晰的情况下有效的去除噪声，但是这种操作比较慢。它拥有着美颜的效果。

      dst = cv2.bilateralFilter(src=image, d=0, sigmaColor=100, sigmaSpace=15)

      src：原图像；
      d：像素的邻域直径，可有sigmaColor和sigmaSpace计算可得；
      sigmaColor：颜色空间的标准方差，一般尽可能大；（越大越模糊）
      sigmaSpace：坐标空间的标准方差(像素单位)，一般尽可能小。(发现越大处理时间越久)

  链接：https://www.jianshu.com/p/6c8db06bb0dc
  链接：https://www.jianshu.com/p/8d11e26c9665

##### 文字区域识别-投影法
https://www.jb51.net/article/178717.htm
##### getStructuringElement()函数
返回指定形状和尺寸的结构元素,一般在调用erode以及dilate函数之前，可根据特定的内核进行直线提取：

![upload successful](\images\pasted-103.png)
https://blog.csdn.net/u013066730/article/details/97242189
https://www.bilibili.com/read/cv2725908/

##### cv2.bitwise_and() 函数
cv2.bitwise_and()是对二进制数据进行“与”操作，即对图像（灰度图像或彩色图像均可）每个像素值进行二进制“与”操作，1&1=1，1&0=0，0&1=0，0&0=0

    OutputArray dst  = cv2.bitwise_and(InputArray src1, InputArray src2, InputArray mask=noArray());//dst = src1 & src2

OutputArray dst  = cv2.bitwise_and(InputArray src1, InputArray src2, InputArray mask=noArray());//dst = src1 & src2

参考：https://blog.csdn.net/Hren0412/article/details/97623740
##### cv2.HoughLines
cv2.HoughLines()函数是在二值图像中查找直线，cv2.HoughLinesP()函数可以查找直线段。

    cv2.HoughLinesP()函数原型：

    HoughLinesP(image, rho, theta, threshold, lines=None, minLineLength=None, maxLineGap=None) 

    image： 必须是二值图像，推荐使用canny边缘检测的结果图像； 
    rho: 线段以像素为单位的距离精度，double类型的，推荐用1.0 
    theta： 线段以弧度为单位的角度精度，推荐用numpy.pi/180 
    threshod: 累加平面的阈值参数，int类型，超过设定阈值才被检测出线段，值越大，基本上意味着检出的线段越长，检出的线段个数越少。根据情况推荐先用100试试
    lines：这个参数的意义未知，发现不同的lines对结果没影响，但是不要忽略了它的存在 
    minLineLength：线段以像素为单位的最小长度，根据应用场景设置 
    maxLineGap：同一方向上两条线段判定为一条线段的最大允许间隔（断裂），超过了设定值，则把两条线段当成一条线段，值越大，允许线段上的断裂越大，越有可能检出潜在的直线段

参考：https://www.cnblogs.com/mtcnn/p/9411774.html
##### 高级形态学变换：
    开运算：
    先腐蚀，再膨胀，可清除一些小东西(亮的)，放大局部低亮度的区域
    闭运算：
    先膨胀，再腐蚀，可清除小黑点
    形态学梯度：
    膨胀图与腐蚀图之差，提取物体边缘
    顶帽：
    原图像-开运算图，突出原图像中比周围亮的区域
    黑帽：
    闭运算图-原图像，突出原图像中比周围暗的区域

原文链接：https://blog.csdn.net/keen_zuxwang/article/details/72768092

#### 问题：
1、读取图片尽量不用中文路径

2、不加waitKey造成程序无响应

3、 在Pycharm中运行cv.imshow()函数的时候，图形界面闪了一下就消失了

原来，在运行cv2.imshow后，需要使用cv2.waitKey来保持窗口的显示。

再添加个cv2.destroyAllWindows()点击enter可自动销毁图片


https://my.oschina.net/u/4070307/blog/3008278