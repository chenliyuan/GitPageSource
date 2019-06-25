title: python版本升级和卸载
date: 2019-06-08 18:06:30
---
Linux安装python2.7、pip和setuptools写的特别详细：
https://www.jianshu.com/p/200c9b9dcac8

 由python2.7升级至python3.x
 linux:
 https://blog.csdn.net/qq_37342157/article/details/81244667  
 mac:
https://www.cnblogs.com/cynthia-wuqian/p/9303514.html

卸载python3 for mac:  
https://www.jianshu.com/p/98383a7688a5

1. 经验：【软连接】
**ln -s建立软链接；不带-s建立的是硬链接
建立软连接：**
```
[root@tv6-hotelqa-newhotel-17 bin]# ln -s /usr/local/python2.7/bin/pip2  /usr/bin/pip
```
建立后效果：
相当于windows的快捷方式，搜索pip的时候相当于直接访问的对应的软链接pip2

==**ln   -s    跳转的目录     快捷键**==
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181227184924613.png)
**删除软连接：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181227185207357.png)
==切记：后面不要加“/”==，尽管知道这个点，但是手一抖删除了其对应软件的所有内容，不长教训不长记性啊

**取消链接**

    [root@tv6-hotelqa-newhotel-17 bin]# unlink python
2. 升级带来的版本问题

==*由python2升级至python3.X,但是python -V(或--version)总还是2.7版本，改了/usr/bin下的python软连各种，都不行（搞了好几个小时，眼快瞎了）*==
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019022815140256.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019022816031164.png)
试着更改环境变量：
echo $PATH查看环境变量
/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
vi /etc/profile   source  /etc/profile 更改环境变量
#### Finally：
觉悟到是不是在别的路径，于是在环境变量中的usr/local/bin中找到了有个同名的python，原来指向的就是2.7.于是删除此软链，果然提示在该目录（即usr/local/bin）目录找不到python,这下狐狸尾巴露出来了吧~~ 于是添加指向到python3的软链，测试终于OK了，功夫不负苦心人到底！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190228162706440.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)