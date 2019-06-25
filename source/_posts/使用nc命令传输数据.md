
title: 使用nc命令传输数据
---
原文：https://www.cnblogs.com/nmap/p/6148306.html
	nc是netcat的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具
## 使用ncp命令传送数据
需注意操作次序，receiver先侦听端口，sender向receiver所在机器的该端口发送数据。  
步骤一：在接收端B侦听端口数据，并将数据写到file里
nc -l port >file
![在接收端侦听端口数据，并将数据写到file里](https://img-blog.csdnimg.cn/20190128143512804.png)
步骤二：在发送端A往接收端B的指定端口发送数据，把文件发送过去
nc ip port < somefile
nc 10.0.1.162 9995 < somefile
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190128143444169.png)

## 安装nc

1、先将已安装的nc删除

yum erase nc

2.下载较低版本的nc的.rpm文件

           wget   http://vault.centos.org/6.6/os/x86_64/Packages/nc-1.84-22.el6.x86_64.rpm

3.安装.rpm文件

rpm   -iUv    nc-1.84-22.el6.x86_64.rpm

原文：https://blog.csdn.net/yctccg/article/details/52218055 



