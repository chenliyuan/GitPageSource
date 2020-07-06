title: nginx使用小记
author: 躲不掉的风
date: 2020-06-19 10:38:15
tags:
---
1. 常用命令：

       nginx -s stop   直接干掉服务
       start nginx     启动服务
       nginx -s quit         优雅停止nginx，有连接时会等连接请求完成再杀死worker进程  
        nginx -s reload     优雅重启，并重新载入配置文件nginx.conf
        nginx -s reopen     重新打开日志文件，一般用于切割日志
        nginx -v            查看版本  
        nginx -t            检查nginx的配置文件
        nginx -h            查看帮助信息
        nginx -V       详细版本信息，包括编译参数 
        nginx  -c filename  指定配置文件

2. 查找和安装：

        sudo apt-get install nginx
        cd /etc/nginx/conf.d
        查找
        find / -name nginx.conf   2>/dev/null
    
3. 没有root权限？
    
    以非root权限安装nginx及运行:
    https://www.cnblogs.com/xmaples/p/5836818.html
    
    测试     
    pgrep -a nginx   
    nginx -c /home/ld-sgdev/liyuan_chen/nginx.conf
    验证   
    curl localhost:8080   
    使用命令：   
    lsof -i :8080

4. 修改nginx重启后未生效？
    很多进程需要逐一杀除
    
    ps -ef|grep nginx|awk '{print $2}'|xargs kill -9
    
5. nginx: [error] open() "/usr/local/var/run/nginx.pid" failed (2: No such file or directory)

	直接启动命令nginx(非重启)，重新生成nginx.pid就可以了：

6. Mixed Content: The page at 'xxx' was loaded over HTTPS, but requested an insecure resource 'xxx'.

 【解决】添加：```<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">```
 
7. mac 使用brew安装后默认地址：

   /usr/local/etc/nginx/nginx.conf 

   默认端口占用8080