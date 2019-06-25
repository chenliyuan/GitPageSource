title: Docker
---
参考菜鸟教程 https://www.runoob.com/docker/docker-tutorial.html

## **1. 启动docker**

 命令行启动 或点击图标app启动
 2. 运行前检查
 $ docker   查看所有命令
 $ docker --version
 $ docker info
 $ docker images  查看当前所有镜像


## **3. 启动容器：docker run imageID**

 –name  容器名称，用一个有意义的名称命名即可。   
  -i -t 保留命令行运行(之后可以输入命令)    
  -p 7777:8888
   7777是宿主机暴露出来的端口号，8888是容器内jupyter服务器的端口号。    
   -d:让容器在后台运行。    
   -v  :   表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>	     
   eg：~/tensorflow:/notebooks/data      
   将本地的~/tensorflow文件夹挂载到新建容器的/notebooks/data下（这样可是实现本地文件和docker文件互通）   
   tensorflow/tensorflow为指定的镜像，默认标签为latest（即tensorflow/tensorflow:latest）
   -w  指定其为工作目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190519154825167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
后台：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019051415473048.png)

			runoob@runoob:~/python$ docker run  -v $PWD/myapp:/usr/src/myapp  -w /usr/src/myapp python:3.5 python helloworld.py
		命令说明：
			-v ：$PWD/myapp:/usr/src/myapp :将主机中当前目录下的myapp挂载到容器的/usr/src/myapp
			-w ：/usr/src/myapp :指定容器的/usr/src/myapp目录为工作目录
			python helloworld.py : 使用容器的python命令来执行工作目录中的helloworld.py文件

## **3. 容器管理**

	 $ docker ps  -a  //使用 docker ps 来查看我们 **正在运行** 的容器
	
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514152454652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)

docker rm +容器名                 //删除容器时，容器必须是停止状态，否则会报错
docker rm $(docker ps -a -q)
docker stop +ID 或者名字
docker start +容器名               //已经停止的容器，我们可以使用命令 docker start 来启动。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514160952467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
其他：
    docker port +（ID 或者名字）//容器的某个确定端口映射到宿主机的端口号
    docker logs [ID或者名字]       //可以查看容器内部的标准输出。
    docker top +容器名                          //查看容器内部运行的进程
    docker inspect +容器名 //查看 Docker 的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。

## **4. 镜像管理**

  **docker images**   //列出本地主机上的镜像 
	REPOSITORY：表示镜像的仓库源
	TAG：镜像的标签(可以理解为不同的版本号)
	IMAGE ID：镜像ID
	CREATED：镜像创建时间
  **docker pull**  获取新镜像
			
		Crunoob@runoob:~$ docker pull ubuntu:13.10
		
我们可以从 Docker Hub 网站来搜索镜像，Docker Hub 网址为： https://hub.docker.com/
我们也可以使用 **docker search** 命令来搜索镜像
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190514164058817.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
ps:
镜像加速
mac:http://f971cfe6.m.daocloud.io加入deamon-Registry mirrors

实例：
1、docker ps或docker images
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
原因：未打开docker，启动docker图标，直至展示running
2、打开后发现需要指定运行目录
ls查看当前目录结构，选择映射目录
再停掉容器：
docker stop  362f8b09c8f43517957423b23f5e0d07ffa164864170262ec14150f2ad267
再打开的时候加-v 参数


