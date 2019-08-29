title: 必知的linux命令
tags: []
categories: []
date: 2019-07-15 19:28:00
---
- vim 有关命令  
	- u撤销  
	- vim -b 用来查看二进制比如结束符 
    - :set num显示行号 ;  
    - :7,10d 清除7到10行的内容；
    - 行定位：G 定位到最后一行；
          1G定位到第一行；
          17G定位到第17行；
    - 复制粘贴：
      yy 复制当前行
      p 粘贴
    - 字符替换命令：
  		:s/old/new/g ==:%s/old/new/g 替换所有

- |：   管道命令  如 cat note.conf |more   
	其中Shift + PageUp 向上查看
- grep：管道配合grep很好组合 如cat -n note.conf | grep head   -n显示行号    
	- grep -i 不区分大小写
- ls ：  -a 所有（all）、-l 列显示(list)、ls -h（大小GB、KB显示,默认是按照字节显示大小的）、
ls -l=ll ll -ht  t标识按照时间倒序
- find：  
    * -name 例：find /home -name  h?llo*;   
    * -size find /home -size +10k
- 重定向命令 >  
  * ls -l /etc  > /home/myback.txt  (覆盖重定向)把显示的结果覆盖到/home/myback.txt中去   
  * ls -l /etc/  >>  /home/myback.txt   追加重定向
  
- 清空文件三种方法
 -   ```> log.txt   清空log.txt文件```
 -  ```echo "" >log.txt```
 -  ```cat /dev/null >log.txt```
 
- 文件颜色     
绿色-> 可执行文件  
蓝色-> 目录   
浅蓝色->链接  
红色->压缩文件  

![upload successful](\images\pasted-64.png)
- lsof(list openfiles)  
	
	eg:  lsof -i :8080  查看所有8080端口的网络连接  
相当于 netstat  nlap |grep -i 8080
- 创建用户以及查看当前用户
useradd redis
passwd  redis
/etc/group 查看所有组
/etc/shadow和/etc/passwd查看所有用户

- 查看内存使用情况
free -m(m为MB,g为GB)
查看磁盘使用情况：df -lh
查看当前目录下所有文件大小  du -sh *

- chown  修改所属用户
chown -R tester:tester /var/www
- chmod  权限修改

    一共有两种语法，一种是chmod ugo+r file 这种
    一种是chmod abc file(其中abc是数字)
    具体来说，abc分别表示User、Group、及Other的权限。其中r=4,w=2,x=1

  - 若要rwx属性则4+2+1=7
  - 若要rw-属性则4+2=6
  - 若要r-x属性则4+1=5
  - 若用chmod 4755（或755） filename可使其具有root权限
 
- 查看某端口连接状态

	netstat -nlap |grep -i 6379
  ```
  -n 拒绝显示别名，能显示数字的全部转化成数字。
  -l 仅列出有在 Listen (监听) 的服務状态
  -a (all)显示所有选项，默认不显示LISTEN相关
  -p 显示建立相关链接的程序名
  ```
  nlap基本能满足一般的查找需求，记住这个组合即可
- cp  
  cp -r 原文件  新文件名  
  -r目录复制
- mv  
对于当前路径下文件重命名，非当前路径移动
-  查看linux内核   
结合使用uname -a 查看linux内核
```
[root@tv6-hotelqa-newhotel-17 ~]# cat /etc/issue
CentOS release 6.4 (Final
```
- ln 软连接和硬链接
```
第一，ln命令会保持每一处链接文件的同步性，也就是说，不论你改动了哪一处，其它的文件都会发生相同的变化；
第二，ln的链接又软链接 和硬链接两种，
		 软链接就是ln -s src  dst,它只会在你选定的位置上生成一个文件的镜像，不会占用磁盘空间，
		 硬链接ln src  dst,没有参数-s, 它会在你选定的位置上生成一个和源文件大小相同的文件，无论是软链接还是硬链接，文件都保持同步变化。 
第三，指向一个文件的所有 硬链接都删掉的话文件的内容才会被删掉
		  软链接只要删掉了源链接文件，软链接也就失效了
```
   
-  **查看linux系统版本**
 cat /etc/issue

![upload successful](\images\pasted-65.png)

- 三大操作系统

![upload successful](\images\pasted-66.png)

- 一般来说著名的**linux系统**基本上分两大类：

      1.RedHat系列：Redhat、Centos、Fedora等 
      2.Debian系列：Debian、Ubuntu等 
      RedHat 系列 
      1 常见的安装包格式 rpm包,安装rpm包的命令是“rpm -参数” 
      2 包管理工具 yum 
      3 支持tar包 
      Debian系列 
      1 常见的安装包格式 deb包,安装deb包的命令是“dpkg -参数” 
      2 包管理工具 apt-get 
      3支持tar包
	
- linux常用目录
/bin  存放二进制可执行文件，常用命令一般都在这里
/etc 存放系统管理和配置文件
/usr/
/opt
/root
/var

- awk '{print $1}' fortest.php |sort|uniq -c|wc -l   
  sort命令排序（默认安卓首字母ASCII排序）  
  uniq常与sort结合使用，去除重复行 -c 显示重复个数  
  wc 计算字数 -l只显示行数
- **about mac操作**   
mac下显示/隐藏文件，shit+cmd+句号
- **安装Yum**   
找了很多资料，什么简洁办法都不管用，还是需要下载很多依赖包安装才行。参考如下
https://blog.csdn.net/zgege/article/details/82315110
但是由于yum的版本与当前python的版本不一致会报错？

    There was a problem importing one of the Python modules required to run yum. The error leading to this problem was:
    
       No module named yum
    
    Please install a package which provides this module, or verify that the module is installed correctly.
    
    It’s possible that the **above module doesn’t match the current version of Python**, which is:
    。。。
解决办法：更改配置python引用指向低版本（更改yum配置，因为其要用到python2才能执行，否则会导致yum不能正常使用）。http://www.cnblogs.com/blueel/archive/2012/08/19/2646127.html

	`vi /usr/bin/yum`
	把#! /usr/bin/python修改为#! /usr/bin/python2