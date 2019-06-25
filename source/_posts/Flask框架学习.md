title: Flask+Django框架学习
author: 躲不掉的风
date: 2019-06-15 17:50:33
tags:
---
示例：
```
from flask import Flask
app=Flask(__name__)

@app.route('/')
def hello_world():
	return "hello_world"
if __name=='__main__':
	app.run()
    
```
环境：mac 10.12.6  pycharm +Flask

【问题1】找不到项目的Flask配置位置？debug模式怎么一直是off，即使run后面添加了debug=True？

Flask配置：点击项目名，在如下位置编辑

![upload successful](/images/pasted-26.png)

![upload successful](/images/pasted-27.png)

【问题2】执行创建数据库表的操作时提示Mysql Error 2003: Can't connect to MySQL server on '127.0.0.1' ([Errno 61] Connection refused)
![upload successful](/images/pasted-28.png)
解决：没有安装Mysql服务，于是下载安装，具体见mysql那一篇