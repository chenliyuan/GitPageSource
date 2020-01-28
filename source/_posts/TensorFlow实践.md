title: TensorFlow实践
author: 躲不掉的风
date: 2019-9-24 15:30:42
---
1. 安装 TensorFlow
系统版本：mac 10.12.6 python版本：2.7
安装指南：TensorFlow中文社区
http://www.tensorfly.cn/tfdoc/get_started/os_setup.html#virtualenv_install
1）尝试了使用直接安装\homebrew，都会提示错误：

```
 There was a problem confirming the ssl certificate: [SSL: TLSV1_ALERT_PROTOCOL_VERSION] tlsv1 alert protocol version (_ssl.c:590) - skipping
```
说是因为python社区已不支持TLS version 1.0 & 1.1，可以通过升级pip实现，但是升级pip还会由于这个问题不能成功，后放弃。但是本机是Python2.7不愿因为安装这个升级python版本，毕竟是几个电脑里唯一一个2.x的版本，希望可以保留。
2）使用docker安装
参考：
https://blog.csdn.net/qq_35624642/article/details/82910641
报错：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322173316121.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
于是试图更改用户名和用户组，查阅各种资料，mac操作用户和用户组跟linux不一样，使用dscl命令https://www.4455q.com/tech/mac-dscl-user-group-create-delete-append.html
查到用户名和用户组id后，使用后：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322173457617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)

查看版本和安装路径
import tensorflow as tf

tf.__version__

查询tensorflow安装路径为:

tf.__path__

-------------------------
docker打开容器运行tensorflow镜像：
    **docker run --name xiaolei-tensortflow -it -p 8888:8888 -v ~/tensorflow:/notebooks/data  tensorflow/tensorflow**

docker run 运行镜像，
--name 为容器创建别名，
-it 保留命令行运行(之后可以输入命令)
-p 8888:8888 将本地的8888端口 http://localhost:8888/ 映射（对应前面的端口），
-v ~/tensorflow:/notebooks/data 将本地的~/tensorflow文件夹挂载到新建容器的/notebooks/data下（这样可是实现本地文件和docker文件互通）
tensorflow/tensorflow 为指定的镜像，默认标签为latest（即tensorflow/tensorflow:latest）