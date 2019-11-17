title: python  OpenCV
author: 躲不掉的风
date: 2019-11-04 20:24:15
tags:
---
#### 常用方法    
    import cv2
    #生成图片
    img=cv2.imread("D:/cly/xuanxiu/img/girl.jpg")
    #生成灰色图片
    imgGray=cv2.imread("D:/cly/xuanxiu/img/girl.jpg",0)
    print(img.shape)#图片高、宽、通道
    print(img.size)#图总像素
    print(img.dtype)#图片数据类型是uint8

    print(imgGray.shape)#图片高、宽、通道；灰色图不返通道
    #操作：中值滤波
    result=cv2.medianBlur(imgGray,3)
    cv2.namedWindow('result',0)#创建窗口
    cv2.resizeWindow('result',300,400)#设置大小
    #显示图片
    cv2.imshow("result",result)
    #等待图片关闭
    cv2.waitKey(0)
    
    #销毁所有窗口
    cv2.destroyAllWindows()
####  图像处理
##### 中值滤波：medianBlur 

优点：中值滤波在一定的条件下可以克服常见线性滤波器如方框滤波器、均值滤波等带来的图像细节模糊，而且对滤除脉冲干扰及图像扫描噪声非常有效，也常用于保护边缘信息, 保存边缘的特性使它在不希望出现边缘模糊的场合也很有用，是非常经典的平滑噪声处理方法。

缺点：但是中值滤波的缺点也很明显，因为要进行排序操作，所以处理的时间长，是均值滤波的5倍以上。

##### 双边滤波 bilateralFilter
是一种非线性滤波方法。能够保持边界清晰的情况下有效的去除噪声，但是这种操作比较慢。它拥有着美颜的效果。

    dst = cv2.bilateralFilter(src=image, d=0, sigmaColor=100, sigmaSpace=15)

    src：原图像；
    d：像素的邻域直径，可有sigmaColor和sigmaSpace计算可得；
    sigmaColor：颜色空间的标准方差，一般尽可能大；（越大越模糊）
    sigmaSpace：坐标空间的标准方差(像素单位)，一般尽可能小。(发现越大处理时间越久)

链接：https://www.jianshu.com/p/6c8db06bb0dc
链接：https://www.jianshu.com/p/8d11e26c9665

#### 问题：
1、读取图片尽量不用中文路径

2、不加waitKey造成程序无响应