title: Linux常用命令篇
date: 2019-06-16 17:30:52
---
-  查看linux内核   
结合使用uname -a 查看linux内核
```
[root@tv6-hotelqa-newhotel-17 ~]# cat /etc/issue
CentOS release 6.4 (Final
```
- 查看端口占用情况

	![upload successful](\images\pasted-36.png)

-  Linux显示tab 空格 换行符
	![upload successful](\images\pasted-39.png)

- Linux cp -r 原文件夹  新文件夹名 

   -r命令可以文件夹整个复制

-  **rpm命令**
RPM是RedHat Package Manager（RedHat软件包管理工具）类似Windows里面的“添加/删除程序”
rpm -qa | grep httpd　　　　　 ＃[搜索指定rpm包是否安装]
  rpm   -e 　　　　　　　　       #移走一个包   
-  **查看linux系统版本**
 cat /etc/issue
 cat /etc/issuecat /etc/issue
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190225194854972.png)
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

-  **yum和apt-get的区别**

  yum和apt-get的区别

      一般来说著名的linux系统基本上分两大类： 
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
	
- **进程管理**  
netstat -nap
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190304192842935.png)

- **清除文件内容命令**  
$ echo "" >log.log

- **chmod和chown文件调用权限  **   
一共有两种语法，一种是chmod ugo+r file 这种  
一种是chmod abc file(其中abc是数字)
具体来说，abc分别表示User、Group、及Other的权限。其中r=4,w=2,x=1

    -  若要rwx属性则4+2+1=7
    -  若要rw-属性则4+2=6
    -  若要r-x属性则4+1=5
    若用chmod 4755 filename可使其具有root权限。
 
 chown
变更目录的所有者：   
      chown -R test:test /tmp/src

- **about mac操作**   
mac下显示/隐藏文件，shit+cmd+句号