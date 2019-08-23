title: python语法
author: 躲不掉的风
date: 2019-08-09 17:29:00
tags:
---
- ##### 装饰器
- 自定义装饰器，常用作打日志和运行时间
  ```
    import time
    def logger(func):
        #如果原函数有参数，那闭包函数必须保持参数个数一致
        def mywrapper(x,y):#将函数外面包一层
            print(x,y)#添加的额外功能,打印日志(1,2)
            t1=time.time()
            func(x,y)#原函数add(a,b)
            t2=time.time()
            cost=t2-t1#额外功能：计算时间
            print("共计花费时间{}".format(cost))
        return mywrapper

    @logger
    def add(a,b):
        print("a+b={}".format(a+b))
        time.sleep(1)
    add(1,2) #相当于logger(add)

  ```  


- ##### 格式化

https://www.cnblogs.com/lvcm/p/8859225.html这篇文章总结的很好

- ##### multiprocessing模块processing
参考:https://blog.csdn.net/qq_25171075/article/details/81871537#%E4%B8%80.multiprocessing%E6%A8%A1%E5%9D%97(%E5%8C%85)
	- 主要参数：   
    target表示调用对象 , 即子进程要执行的任务  
    args表示调用对象的位置参数,元组类型。  
	- 方法介绍：    
    p.start() : 启动进程,并调用子进程中的p.run ()  
    p.is_alive () : 如果p仍然运行 , 返回True

  ```
  import multiprocessing
  import os

  def run_proc(name):
      print('Child process {0} {1} Running '.format(name, os.getpid()))

  if __name__ == '__main__':
      print('Parent process {0} is Running'.format(os.getpid()))
      for i in range(5):
          p = multiprocessing.Process(target=run_proc, args=(str(i),))
          print('process start')
          p.start()
      p.join()
      print('Process close')
  ```

- ##### 处理小数  
参考：https://www.jb51.net/article/102248.htm
  - 向下取整 int()
  - 四舍五入 round()
  ```
  >>> round(3.25); round(4.85)
  3.0
  5.0
  ```
  - 向上取整 ceil()
  - 分别取整数部分和小数部分
  ```
  >>> import math
  >>> math.modf(3.25)
  (0.25, 3.0)
  >>> math.modf(3.75)
  (0.75, 3.0)
  >>> math.modf(4.2)
  (0.20000000000000018, 4.0)
  ```