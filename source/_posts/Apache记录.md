title: Apache记录
author: 躲不掉的风
date: 2019-06-26 13:16:54
tags:
---
service httpd status
service httpd start/restart 
service httpd  stop

查找httpd.conf配置文件的存放路径
find /etc/httpd/  -name *conf

![upload successful](\images\pasted-54.png)

更改端口号，在查找到的httpd.conf文件里找到Listen
可以看到默认是80，修改成你想换的不用端口就好啦，查询端口是否占用：
lsof -i :8080

然后原来的php网址在原来ip后加上8080端口就可以正常访问啦