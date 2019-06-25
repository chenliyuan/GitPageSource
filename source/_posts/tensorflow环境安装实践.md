title: tensorflow环境安装实践
author: 躲不掉的风
tags: []
categories: []
date: 2019-06-08 20:27:00
---

环境选择：
python2.7、mac 
原因：  
1、有个县城的项目是基于python2写的，希望可以运行。  
2、参考此网站说明支持2.7，未提及3.x(可能还没更新)

##### 弯路1：
参考tf中文社区下载和安装步骤：http://www.tensorfly.cn/tfdoc/get_started/os_setup.html
一共提供了四种方式，最后选择了从源码安装
* 二进制安装
* 基于 Docker 的安装：   
   *之前试了很久也没成功过，再加上没找到python2.7的镜像*
* 基于 VirtualEnv 的安装
* 从源码安装
安装 Bazel、SWIG（依赖PCRE）、Numpy，然后安装，最后一步失败了。

![upload successful](/images/pasted-0.png)
#### 弯路2：
探索使用二进制安装中提供的这个命令直接使用（是virtualenv 中的）：
$ pip install https://storage.googleapis.com/tensorflow/mac/tensorflow-0.5.0-py2-none-any.whl
结果果然不行

![upload successful](/images/pasted-1.png)

### 成功案例
于是更改方案，找tf官网：https://tensorflow.google.cn/

选择pip安装方式
1. 在系统上安装 Python 开发环境
2. 创建虚拟环境（推荐
3. 安装 TensorFlow pip 软件包

选的是3中按系统安装：  
 pip install --user --upgrade tensorflow  
 
 但是无论怎样，都提示pip版本有问题
 
![upload successful](/images/pasted-2.png)

于是安装pip：  
直接按照提示升级安装？结果不行  
pip install pip? 还是不行  
下载源码安装？OK，安装日志大概20页。。
![upload successful](/images/pasted-3.png)

然后再次安装tensorflow，第一次超时，第二次成功了（很慢，需要耐心等待）

