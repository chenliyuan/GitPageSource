title: tensorflow环境安装实践2
author: 躲不掉的风
date: 2019-06-15 17:03:19
tags:
---
由于之前是在本机的python2.7基础上安装的本次希望安装个基于3.x的，为了兼容其他同学的环境代码。   
方案：使用虚拟隔离环境构建  
问题1：在本机安装py3.7的包，然后在已经有的virtualenv（之前在py2.7下安装的）下链接此pyh环境提示：
New python executable in /Users/cly/Documents/my_env/bin/python3  

没办法解决，愤然上课去，下课后心平气和的重新用新装的pip3.7重新装了一遍virtualenv，这个问题解决:
```
MacBook-Pro:bin cly$ pip3 install -U virtualenv
MacBook-Pro:workspace cly$ virtualenv -p /Library/Frameworks/Python.framework/Versions/3.7/bin/python3  my_env
MacBook-Pro:workspace cly$ source my_env/bin/activate  #激活，开始使用
(my_env)MacBook-Pro:workspace cly$ python -V#注意目录前面出现(my_env)
Python 3.7.3
(my_env)MacBook-Pro:workspace cly$ ：deactivate 退出
```