title: 必知的linux命令
tags: []
categories: []
date: 2019-07-15 19:28:00
---
#### cut命令

    -b 以字节为单位分割
    -c以字符为单位分割
    -d 自定义分隔符
    -f与-d一起使用，指定哪一列
  如：以冒号为分隔符，取第四列相当于awk -F: '{print $4}'
  
  	cat /etc/passwd|cut -d: -f4
        
#### /etc/passwd字段
/etc/passwd中一行记录对应着一个用户，每行记录又被冒号(分隔为7个字段，其格式和具体含义如下：

    /etc/passwd
	 用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
    
    # cat /etc/group
	组名：加密密码：组ID：所有属于该组的用户。
##### 替换sed

  替换掉每行
  第一个原字符串 sed -i 's/原字符串/新字符串' filename
  
  替换掉所有的字符串 sed -i 's/原字符串/新字符串/g' filename
  
  	-i 的作用是直接作用于(修改)文件，而不加-i只是在终端打印结果
    
       # 对每行匹配到的第一个字符串进行替换
      sed -i 's/原字符串/新字符串/' ab.txt 

      # 对全局匹配上的所有字符串进行替换
      sed -i 's/原字符串/新字符串/g' ab.txt 

      # 删除所有匹配到字符串的行
      sed -i '/匹配字符串/d'  ab.txt  

      # 特定字符串的行后插入新行
      sed -i '/特定字符串/a 新行字符串' ab.txt 

      # 特定字符串的行前插入新行
      sed -i '/特定字符串/i 新行字符串' ab.txt

      # 把匹配行中的某个字符串替换为目标字符串
      sed -i '/匹配字符串/s/源字符串/目标字符串/g' ab.txt

      # 在文件ab.txt中的末行之后，添加bye
      sed -i '$a bye' ab.txt   

      # 对于文件第3行，把匹配上的所有字符串进行替换
      sed -i '3s/原字符串/新字符串/g' ab.txt
      
	  # 显示文件的第二行到最后一行
      sed -n '2, $p' ab.txt        

原文链接：https://blog.csdn.net/yjk13703623757/article/details/79548450

- awk '{print $1}' fortest.php |sort|uniq -c|wc -l  

    sort命令排序（默认安卓首字母ASCII排序）  
    uniq常与sort结合使用，去除重复行 -c 显示重复个数  
- **wc 命令**
计算行数、单词数、字节数。只有三个参数：

  -l 只显示行数（lines）  
  -w 只显示单词数(words)  
  -c 只显示字节数(chars)
  
##### 查看网络连接状态(路由表)
 可以查端口号，而ps无端口号
 
	netstat -nlap |grep -i 6379
  ```
  -n 拒绝显示别名，能显示数字的全部转化成数字。
  -l 仅列出有在 Listen (监听) 的服務状态
  -a (all)显示所有选项，默认不显示LISTEN相关
  -p 显示建立相关链接的程序名
  ```
  nlap基本能满足一般的查找需求，记住这个组合即可 
  
##### 查看进程 ps   
常用两个组合：

     ps -ef |grep -v grep |grep tomcat
     ps -aux
     类似于lsof -i :8080       
     #lsof (list openfiles)  相当于 netstat  nlap |grep -i 8080
 
- Linux 下命令有哪几种可使用的通配符？分别代表什么含义?

      ?可替代单个字符。
      *可替代任意多个字符。
      方括号[charset]可替代 charset 集中的任何单个字符，如[a-z]，[abABC]
![upload successful](\images\pasted-68.png)
##### tar命令
```
tar -czvf  test.tar.gz a.c  #将a.c文件压缩成test.tar.gz
tar  -xzvf test.tar.gz      #解压缩
```
参数说明：
-t 查看内容    
-c 建立压缩文档   
-v 显示所有过程   
-f 后面接要处理的文件,最后用  
-x 解压缩  
-z 有gzip属性  

#####  vim 常用命令 

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
#####  查找命令： 

	find:

      -name 例：find /home -name  h?llo;   
      -size find /home -size +10k
    
   whereis: 二进制文件，说明文档，源文件等
   which :可执行文件
   
- 重定向命令 >  
  * ls -l /etc  > /home/myback.txt  (覆盖重定向)把显示的结果覆盖到/home/myback.txt中去   
  * ls -l /etc/  >>  /home/myback.txt   追加重定向
  
- 清空文件三种方法
  
 1. 》 log.txt   清空log.txt文件(单箭头)  
 2. echo "" >log.txt  
 3. cat /dev/null >log.txt



- 创建用户以及查看当前用户

  useradd redis  
  passwd  redis  
  /etc/group 查看所有组  
  /etc/shadow和/etc/passwd查看所有用户

- 查看内存使用情况

  free -m(m为MB,g为GB)  
  查看磁盘使用情况(disk free)：df -lh  
  查看当前目录下所有文件大小  du -sh *   

#####  chown  修改所属用户
chown -R tester:tester /var/www
##### chmod  权限修改两种语法
 ###### 一种是chmod ugo+r file 这种
 
         命令格式：chmod {u|g|o|a}{+|-|=}{r|w|x} filename 
          u (user)   表示用户本人。 
          g (group)  表示同组用户。 
          o (oher)   表示其他用户。 
          a (all)    表示所有用户。 
          +          用于给予指定用户的许可权限。 
          -          用于取消指定用户的许可权限。 
          =          将所许可的权限赋给文件。 
          r (read)   读许可，表示可以拷贝该文件或目录的内容。 
          w (write)  写许可，表示可以修改该文件或目录的内容。 
          x (execute)执行许可，表示可以执行该文件或进入目录。

  例如：   
  
          # chmod a+rx filename 
            让所有用户可以读和执行文件filename。 
          # chmod go-rx filename 
            取消同组和其他用户的读和执行文件filename的权限。 
          # chmod 741 filename 
            让本人可读写执行、同组用户可读、其他用户可执行文件filename。 
          # chmod -R 755 /home/oracle 
    递归更改目录权限，本人可读写执行、同组用户可读可执行、其他用户可读可执行 


   ###### 一种是chmod abc file(其中abc是数字)
    
   具体来说，abc分别表示User、Group、及Other的权限。其中r=4,w=2,x=1

    - 若要rwx属性则4+2+1=7
    - 若要rw-属性则4+2=6
    - 若要r-x属性则4+1=5
    - 若用chmod 4755（或755） filename可使其具有root权限

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


- 文件颜色     
  绿色-> 可执行文件  
  蓝色-> 目录   
  浅蓝色->链接  
  红色->压缩文件  

![upload successful](\images\pasted-64.png)   
##### linux上安装软件
      1）解压缩，如tar.gz格式的解压缩命令为tar -xzvf soft.tar.gz 
      2） 执行“./configure”命令为编译做好准备；
      3） 执行“make”命令进行软件编译；
      4） 执行“make install”完成安装；
      5） 执行“make clean”删除安装时产生的临时文件。
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