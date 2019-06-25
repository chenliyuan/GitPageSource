title: mysql数据库
author: 躲不掉的风
date: 2019-06-16 12:36:57
tags:
---
使用pymysql的时候创建表不成功，提示连不到，于是才意识到本机没有安装Mysql服务，步骤：  
1、下载https://dev.mysql.com/downloads/mysql/
2、安装的时候设置root密码  
3、设置环境变量  
打开终端,根目录：open .bash_profile 
直接输入如下语句：export PATH=${PATH}:/usr/local/mysql/bin
4、终端登录  mysql -u root -p   
5、手动创建数据库 语句参考https://www.cnblogs.com/zhuyongzhe/p/7686105.html   
6、执行创建表操作（本地写的create.py-连数据库和创建表操作）  
执行后提示Warning: (3719, "'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.")
  result = self._query(query)
忽略即可，表已创建完毕