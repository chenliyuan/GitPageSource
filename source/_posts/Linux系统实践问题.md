title: Linux系统实践问题记录
author: 躲不掉的风
date: 2020-02-28 14:18:35
tags:
---
1. 记录一次在Linux（Ubuntu）中date命令无效的情况

	解决方案参考http://blog.sina.cn/dpool/blog/s/blog_413d250e0102z2iy.html

        1、sudo timedatectl set-ntp false
        2、sudo date -s '2020-2-20 12:12:12'
        此时通过命令查看时间的话，就可以发现日期修改成功了，而如果需要将时间同步回去，直接执行 sudo timedatectl set-ntp true即可。
   
  更改时区：tzselect命令
  
  参考：https://blog.csdn.net/zhengchaooo/article/details/79500032