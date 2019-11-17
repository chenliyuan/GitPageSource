title: jmeter实践笔记
author: 躲不掉的风
date: 2019-10-16 14:10:29
tags:
---
#### 1.安装
安装参考：https://blog.csdn.net/lovesoo/article/details/78579547

线程数：可以理解为并发数，在jmeter中一个线程代表一个用户。

Ramp-Up Period (in seconds): 多长时间内初始化完这些线程。单位是秒。我这里设置的是10秒启动100个也就是1秒启动10。

循环次数：如果你要限定循环次数为10次的话，就取消永远前面的那个勾，然后在后面的文本框里面填写10；在这里我们勾上永远，表示如果不停止或者限定时间将会一直执行下去， 是为了方便调度器的调用。一般勾选永远后要在调度器里填写持续时间。

对于结果和图表，原生三个元件常用：聚合报告、图表结果、响应时间图

#### 2.插件使用
第一步：下载jmeter-plugins，https://jmeter-plugins.org/install/Install/，安装提示将jar包放在jmeter安装目录lib/ext下  

第二步：jmeter中option-jmeter plugins Manager-Available Plugins-jdbc Standard Set,点击右下角的Apply Changes and Restart JMeter

