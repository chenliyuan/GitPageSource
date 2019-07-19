title: Django+uwsgi+supervisor+nginx环境部署
author: 躲不掉的风
date: 2019-06-25 20:10:48
tags:
---
理论知识
=======
1. Apache  
Apache是世界使用排名第一的Web服务器软件。它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。版本查看：
```
[root@tv6-hotelqa-newhotel-17 ~]# apachectl -v
Server version: Apache/2.2.15 (Unix)
Server built:   Oct 19 2017 16:43:38
```

2. FQDN 是Fully Qualified Domain Name的简写，意思是完整的域名。

  FQDN=主机名（hostname）+域后缀
  如 主机名是 fanyi 域名是 baidu.com
  FQDN= fanyi.baidu.com
  如何查看主机名？
  设置主机名： vi /etc/hostname
  第一个字段是该主机的IP地址, 第二个和第三个字段是你希望设置的主机名,最后是刚刚设置的FQDN（这两个顺序应该没关系）:
  ![upload successful](\images\pasted-45.png)
  hostname -F /etc/hostname更新主机名
  hostname -f看到主机名

- 路径
 
 centos6x  nginx默认一般在/etc/nginx路径下
 httpd.conf 一般在/etc/httpd 文件夹下的子文件里 使用 (find  +地址 -name  +名称)查找即可
 
 
 
 
 
搭建过程
===========
最后决定使用 nginx + uwsgi socket 的方式来部署 Django，因为听说是比 Apache mod_wsgi 要复杂一些，但这是目前主流的方法。环境：Centos6 参考：https://code.ziqiangxuetang.com/django/django-nginx-deploy.html  
其他：  
https://www.jianshu.com/p/36ef6557c910  

1、安装nginx   
sudo yum install epel-release   
sudo yum install python-devel nginx  
ps -e | grep nginx查看是否已经启动了nginx
nginx -t 检查语法
nginx -s reload
nginx -c /etc/nginx/nginx.conf
nginx -V 版本

2、安装 supervisor, 一个专门用来管理进程的工具，我们用它来管理 uwsgi 进程  
sudo pip install supervisor

3、将 SELinux 设置为宽容模式，方便调试：
```
sudo setenforce 0  
service iptables stop
```
getenforce 或者sestatus -v可以查防火墙状态   

![upload successful](\images\pasted-34.png)
4、安装 uwsgi  
sudo pip install uwsgi --upgrade

5、使用 uwsgi 运行项目
uwsgi --http :8001 --chdir /var/www/html/newworkbench/pycharmpj --home=/path/to/env --module project.wsgi
这样就可以跑了，--home 指定virtualenv 路径，如果没有可以去掉。project.wsgi 指的是 project/wsgi.py 文件

![upload successful](\images\pasted-37.png)

6、使用supervisor来管理进程  
安装 supervisor 软件包    (sudo) pip install supervisor   
7、 新建一个 uwsgi.ini 编辑：
```
[uwsgi]
socket =/tmp2/pycharmpj.sock
chdir =  /var/www/html/newworkbench
wsgi-file = pycharmpj/wsgi.py
touch-reload = /var/www/html/reload
processes = 2
threads = 4
chmod-socket = 664
#chown-socket = tu:www-data
vacuum = true#//退出、重启时清理文件

```
在根目录/下新建tmp2替代原来的tmp(tmp目录可能找不到)
编辑 supervisor.conf，将原来的tmp目录都改成tmp2，然后在最底部添加（每一行前面不要有空格，防止报错）：
```
[program:nwk]
command=uwsgi --ini   /var/www/html/uwsgi.ini
directory=/var/www/html/newworkbench/pycharmpj
startsecs=0
stopwaitsecs=0
autostart=true
autorestart=true

```
启动 supervisor
```
(sudo) supervisord -c /etc/supervisord.conf 重启 zqxt 程序（项目）：
(sudo) supervisorctl -c /etc/supervisord.conf restart zqxt#启动，停止，或重启 supervisor 管理的某个程序 或 所有程序：
(sudo) supervisorctl -c /etc/supervisord.conf [start|stop|restart] [program-name|all]
ps -ef | grep supervisord #查看状态
```
重启后提示unix:///tmp2/supervisor.sock no such file？原因是没有启动过，何谈重启，需要先启动：   
![upload successful](\images\pasted-44.png)

8、出现问题：重启的时候报错

![upload successful](\images\pasted-41.png)
解决过程：
1）将socket目录改为原tmp同路径（怕因为是这个路径太长的缘故）
2）将touch-reload和uwsgi.ini路径都直接放在了/var里
都没解决，最后想到应该学会灵活的解决问题，于是从报错文件开始抓起，打开文件锁定代码后打印分别打印出self.socketfile和源头serverurl，发现从serverurl就错了（打印编辑时用到vim里查看空格问题，如后文记录）
![upload successful](\images\pasted-43.png)
本来还以为是系统定义的serverurl有问题，后来查看cat /etc/supervisord.conf，发现这里不知道什么时候多出来的这些东西。
![upload successful](\images\pasted-40.png)

9、 配置ng后测试不ok，待续
nginx  conf文件配置错误？
```
[root@tv6-hotelqa-xxxxx-17 newworkbench]# nginx  -s reload
nginx: [error] invalid PID number "" in "/var/run/nginx.pid"
```
查知先nginx -c，但
```
[root@tv6-hotelqa-xxxxx-17 newworkbench]# nginx -c /etc/nginx/nginx.conf
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use
```
定位过程：
- 使用service iptables status 查看防火墙的状态，果是开启的状态就使用service iptables stop来关闭。
- 查看占用进程

  1.先使用ps -e | grep nginx查看是否已经启动了nginx

  2.如果没有的话则按照提示，查看0.0.0.0:80端口谁占用了，使用netstat -ltunp（或 lsof -i :80）命令，可以看到

  可以看到0.0.0.0:80端口被httpd这个进程占用了（也就是apcache占用），杀掉重复进程

   ![upload successful](\images\pasted-46.png)
本来不想通过杀进程的方式来解决的，因为有个进程不太了解，怕误删，但是后来没有找到突破口，反正是测试机就暂时杀掉吧，后来发现这个进程跟我启动nginx相关，只要启动这个进程就有，所以就不管啦，把其他占用端口的httpd都杀掉果然奇迹般的好了。

![upload successful](\images\pasted-47.png)

![upload successful](\images\pasted-48.png)
lsof -i :80 | grep httpd |grep -v grep |xargs kill -9
其中grep -v grep是去掉grep查找的进程，xargs意思是将前面获取到的进程作为后面的参数kill掉

10、运行时出错：  ** Nginx的Permission denied错误**  
- 把user选项从nginx改成roothttps://www.cnblogs.com/zhangqunshi/p/6654866.html  
还是不行？

- 创建属于nginx组另外用户www-data:
![upload successful](\images\pasted-49.png)最后还是不行

- 创建一样的用户组和用户名
  打算重新整理权限和路径，把pycharmpj.sock和reload、uwsgi.ini都放在了项目目录里，并且赋予了项目目录一个新用户组和权限（其中pycharmpj.sock权限最大，忘了自己给的还是uwsgi.ini生的效），当然相应的更改引用文件路径。最后在nginx里配置的user使用root

  ![upload successful](\images\pasted-53.png)
出现的问题：   
  【pycharmpj.sock出不来？】  
  这个文件是执行uwsgi命令自动生成的，而supervisor是用来管理它的，重启它就行啦，步骤：  
  1)停止 supervisorctl -c /etc/supervisord.conf stop all
  查看ps -ef | grep supervisord若有重复，删除重复
  ![upload successful](\images\pasted-51.png)
  2)清空（原路径tmp2下）用来存放supervisor.sock的文件夹   
  3)重启supervisor ：supervisorctl -c /etc/supervisord.conf restart  nwk
  可见pycharmpj.sock和tmp2下的supervisor.sock等文件都出来了。

  【nginx -t语法检测失败报错?】  
  nginx: [emerg] invalid host in upstream "/var/www/html/newworkbench/pycharmpj.sock" in /etc/nginx/conf.d/nwk.conf:15
  解决：原因是在这少加了unix，真是一点都不能差啊
  ![upload successful](\images\pasted-52.png)

11、终于出来Django的页面啦，虽然还是有错。  
    You're seeing this error because you have DEBUG = True in your Django settings file. Change that to False, and Django will display a standard 404 page.   
    于是按照提示把setting文件改了，但是该页面无动于衷，还是报这个加上
    The current path, myworkbench/links.php, didn't match any of these.
    后来醒悟到是我没有加路由，加上了之后果然出现啦，太开心啦  
12、但是静态页面还没出来，需要配置一下。
刚开始稍微饶了一点弯路，不过还好，最后解决很简单，更改nginx配置文件增加，重启下nginx后就好了：
```
 location /static {
        alias  /var/www/html/newworkbench/static;
    }
```
13、更改Django下路由url.py，更改链接后重启supervisorctl即生效。（记得改一个链接的时候要把所有它下一级的url都改全，否则部分还是会报错）。  
14、static文件访问不了？
![upload successful](\images\pasted-55.png)
原因：原来动了setting里设置静态文件的路径，多了个斜杠，去掉还原就好了。
![upload successful](\images\pasted-56.png)
15、关于部署   
- . 直接覆盖安装相关文件或文件夹，更改所属人为已设角色和用户(后台登录后报500错误就是这个原因);再重新启动服务即可（setting文件debug:false）。   
supervisorctl -c /etc/supervisord.conf restart nwk

-  数据库被覆盖？
尝试不覆盖migrations文件夹