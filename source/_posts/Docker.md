title: Docker使用篇
date: 2019-10-07 16:24:49
---
参考菜鸟教程 https://www.runoob.com/docker/docker-tutorial.html
https://www.cnblogs.com/hellangels333/p/9749905.html
####  启动docker

 命令行启动 或点击图标启动app
 
 $ docker   可查看所有命令
 
 比如常用命令：
 
   ```
     $ docker version
     $ docker info

  ```
  
#### 镜像管理
- docker search 命令来搜索镜像

ps:镜像加速 mac:http://f971cfe6.m.daocloud.io加入deamon-Registry mirrors

- docker pull  获取新镜像
	
  注意使用OFFICIAL的官方镜像
  
	我们可以从 Docker Hub 网站来搜索镜像，Docker Hub 网址为：
https://hub.docker.com/

	使用国内镜像参考地址：https://www.docker-cn.com/registry-mirror

- docker images   //列出本地主机上的镜像 

      REPOSITORY：表示镜像的仓库源
      TAG：镜像的标签(可以理解为不同的版本号)
      IMAGE ID：镜像ID
      CREATED：镜像创建时间

  实例：

  1、docker ps或docker images

  Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

  原因：未打开docker，启动docker图标，直至展示running

  2、打开后发现需要指定运行目录

  ls查看当前目录结构，选择映射目录
  再停掉容器：
  docker stop  362f8b09c8f43517957423b23f5e0d07ffa164864170262ec14150f2ad267
  再打开的时候加-v 参数
  
#### 容器管理
可通过docker --help 查看所有命令，常用以下几个命令

	 $ docker ps  -a  //使用 docker ps 来查看我们正在运行的容器
	
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514152454652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)

	docker rm +容器名   //删除容器时，容器必须是停止状态，否则会报错
    docker rm $(docker ps -a -q)
    docker stop +ID 或者名字
    docker start +容器名 //已经停止状态的容器，我们可以使用命令 docker start 来启动。
    docker port +（ID 或者名字）//容器的某个确定端口映射到宿主机的端口号
    docker logs [ID或者名字] //可以查看容器内部的标准输出。  
    docker top +容器名  //查看容器内部运行的进程
    docker inspect +容器名 //查看 Docker 的底层信息。它会返回一个JSON 文件记录着 Docker 容器的配置和状态信息。
    
####  启动容器

如果把镜像看成面向对象中的 类 的话，那么容器就是 类 的实例化 对象。

###### 查看运行帮助：

	$ sudo docker run --help
  

###### 举例：启动 ngnix 容器

    $ sudo docker run --name some-nginx -d -p 8080:80 registry.docker-cn.com/library/nginx
	b5bbf1dfe86a21d641a161c05598c0f4f4d4b32fc8d756b6fdf306295067625f

-name 指定启动容器的名称为 some-nginx。

-d 让Docker容器在后台以守护态（Daemonized）形式运行。

-p 将容器的80端口映射到主机的8080端口

registry.docker-cn.com/library/nginx 为启动容器的镜像。


处理过程： <font color=red>
浏览器 –> ubuntu(8080) –> Nginx容器(80)</font>

###### 其他参数说明
  -t （tty）选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上。
  
  -i (interactive)则让容器的标准输入保持打开,即保持交互模式。 
   
  -v  :   表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>	     
   eg：~/tensorflow:/notebooks/data      
   将本地的~/tensorflow文件夹挂载到新建容器的/notebooks/data下（这样可是实现本地文件和docker文件互通）   
   tensorflow/tensorflow为指定的镜像，默认标签为latest（即tensorflow/tensorflow:latest）
   
   -w  指定其为工作目录


	runoob@runoob:~/python$ docker run  -v $PWD/myapp:/usr/src/myapp  -w /usr/src/myapp python:3.5 python helloworld.py
		命令说明：
			-v ：$PWD/myapp:/usr/src/myapp :将主机中当前目录下的myapp挂载到容器的/usr/src/myapp
			-w ：/usr/src/myapp :指定容器的/usr/src/myapp目录为工作目录
			python helloworld.py : 使用容器的python命令来执行工作目录中的helloworld.py文件


