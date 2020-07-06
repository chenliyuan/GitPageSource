title: Selenium&Docker使用
author: 躲不掉的风
date: 2020-01-31 22:15:05
tags:
---
#### Docker  API
原文：https://www.missshi.cn/api/view/blog/5a63285f0a745f6335000008

在Docker的生态系统中，存在下列三种API：

  1. Reistry API：与存储Docker镜像的Registry相关的功能。
  
  2. Docker Hub API：与Docker Hub相关的功能
  
  3. ** Docker Remote API：与Docker守护进程相关的功能**

#### Docker Remote API

  以下可以得到类似的docker info时的JSON格式的数据
  
  	curl http://10.129.144.66:2375/info
  
  调用/images/json接口可以获取**镜像**列表：

      curl http://example.com:2375/images/json | python -mjson.tool


  调用/containers/json接口可以获取正在运行中的**容器**列表：

      curl http://example.com:2375/containers/json | python -mjson.tool
      

  其中python -mjson.tool可以将JSON数据格式化显示。
  
  
#### Python调用Docker API
对于原生Docker Remote API而言，虽然功能很强大，但是使用起来并不是十分的方便。

为了简化Docker Remote API的使用，Python针对Docker API进行了封装，提供了一套完整的Python第三方库：docker，专门用于操作调用Docker服务。

可通过pip安装

	pip install docker
    
创建一个docker_api.py的文件，修改文件内容如下：

    import docker
    client = docker.DockerClient(base_url='tcp://example.com:2375', version="auto")
    client.containers.run("ubuntu", "echo hello world")

client.containers.run相当于docker run，参数ports字典类型，端口映射 {8000: 80}表示容器的8000端口映射到宿主机的80端口

对于client.containers.run的函数的返回值，当detach=False时，返回值是运行过程中的日志，而当detach=True时，返回值是**Container对象**。


####  Selenium Grid

  本文参考 https://www.cnblogs.com/hellangels333/p/9749905.html
  https://www.cnblogs.com/nanaheidebk/p/10109013.html

  具体可看原文，原文更加详细，以下为部分摘录：

  这里主要针对的是 Selenium Grid，它用于分布式自动化测试，就是一套Selenium 代码可在不同的环境上运行。刚好，Docker可快速的创建各种环境。

###### Selenium Grid 有两个概念

- hub ：主节点，你可以看作 “北京总公司的测试经理”。

- node：分支节点，你可以看作 “北京总公司的测试小兵A” 和 “上海分公司的测试小兵B”，还有 “深圳分公司的测试小兵C” …。

  也就是说在Selenium Grid中只能有一个主hub，但可以在本地或远程建立 N 多个分支node，测试脚本指向主hub，由主hub 分配给本地/远程node 运行测试用例。node有两种，一个是firefox，一个chrome。   

    hub：selenium/hub  
    node：selenium/node-firefox ， selenium/node-chrome  

###### docker selenium 环境安装


1. 下载主hub镜像（北京总公司的测试经理）

		$ sudo docker pull selenium/hub

2. 下载主node chrome 镜像（上海分公司的测试小兵B）

		$ sudo docker pull selenium/node-chrome

3. 启动主hub容器

		$ sudo docker run -d -P --name selenium-hub selenium/hub

 	-P 表示 Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口。   
    -d 表示在后台运行     
    -name  命名   
    selenium/hub：运行的镜像名称  

4. 启动分支node chrome 容器

		$ sudo docker run -d --link selenium-hub:hub selenium/node-chrome

	–link 通过 link 关联 selenium-hub 容器，并为其设置了别名hub，链接别名是selenium-hub的容器(即上面启动的容器)     

5. 查看容器

  ```
    LiyuanChendeMacBook-Pro:~ liyuanchen$ docker ps
  CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                     NAMES
  e96d18013f73        selenium/node-chrome   "/opt/bin/entry_poin…"   4 seconds ago       Up 3 seconds                                  eager_sammet
  b9449483ccb5        selenium/hub           "/opt/bin/entry_poin…"   2 minutes ago       Up 2 minutes        0.0.0.0:32768->4444/tcp   selenium-hub                             eloquent_gates
  ```

  Selenium/hub 容器的端口号为 4444，而对Ubuntu映射的端口为 32768，前面通过 -P 参数自动分配。

 工作原理：

    <font color='red'/>selenium Grid脚本 -> ubuntu(32768) -> Hub容器(4444) -> Node Chrome 容器</font>

  最终得到可使用的hub链接是：http://127.0.0.1:32768/wd/hub
    
 ###### 本地实践
 1. 启动主hub容器，获取端口号（按照如上命令）
 2. 启动分支node chrome 容器（按照如上命令）
 3. 使用HUB执行RT命令运行robot命令
 
 			robot -d Output --loglevel TRACE -v env:staging -v area:sg  -v HUB:http://127.0.0.1:32769/wd/hub   -i all -i sg  SellerCenterUI/TestCase/03HomePage/
            
 若改成使用 Remote API 启动容器呢？
 
###### Docker SDK for Python

官方文档：https://docker-py.readthedocs.io/en/stable/index.html

众所周知，Docker向外界提供了一个API来管理其中的资源。这个API可以是socket文件形式的（一般也是默认的，在/var/run/docker.sock中），也可以是TCP形式的,本次项目使用的TCP方式定义




