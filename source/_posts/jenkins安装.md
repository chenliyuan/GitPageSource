title: Jenkins安装和配置
author: 躲不掉的风
date: 2020-01-28 15:13:46
tags:
---
###### Ubuntu安装Jenkins

参考：
https://www.jianshu.com/p/845f267aec52
https://www.cnblogs.com/shuoer/p/9471839.html

```
#添加官方软件仓库的秘钥到本地的apt秘钥中
 wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
 
echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list

sudo apt-get update
sudo apt-get install jenkins
```
安装成功后打开服务
```
root@sz-dev-77-89:/var/lib/dpkg# /etc/init.d/jenkins  start
Correct java version found
[ ok ] Starting jenkins (via systemctl): jenkins.service.
root@sz-dev-77-89:/var/lib/dpkg# systemctl status jenkins

```
安装时若报错：


![upload successful](/images/pasted-74.png)

使用操作文件/var/lib/dpkg/info解决：
```
    cd /var/lib/dpkg/
    mv info/ info_bak         
    mkdir info                 
    apt-get update            
    apt-get -f install         
    mv info/* info_bak/        
    rm -rf info                
    mv info_bak info 
```
######  jenkins节点配置
1. 在jenkins设置-节点管理中添加新节点，主要配置项如下（以90机器为例）：
 ![upload successful](/images/pasted-109.png)
2. 添加节点后将如下代理链接在90机器浏览器中打开，点击Lunch按钮下载slave文件
![upload successful](/images/pasted-108.png)
3. 在90机器成功下载Slave后，可放在桌面打开即启动服务。

###### Jenkins安装git插件

安装jenkins时候安装git插件失败，需在本地先安装git
```
apt-get install git
```
然后在插件管理中选中git安装
###### Jenkins安装配置git

![upload successful](/images/pasted-75.png)

【问题】配置git用户时一直报错：Jenkins Host key verification failed

1.需要在jenkins路径下生产公钥和私钥

2.把所属用户改成jenkins

具体步骤   参考https://blog.csdn.net/hursing/article/details/90521031

1）ssh-keygen -t rsa -C jenkinsServerOnLinux@shopeemobile.com     

-c是备注；-f 指定密钥的文件名  ;-t 是type用于指定算法一般rsa;
在过程中提示生成路径可以直接改成/var/lib/jenkins/.ssh/id_rsa，其他直接回车即可

2）生成的公钥放在gitlab下    
3）按照jenkins提示执行命令
 git ls-remote -h ssh://gitlab@git.garena.com:2222/autotest/scqa_ui_auto_platform_projects.git HEAD

根据提示直接yes就好，此步骤会生产know_hosts的文件如果在根路径/root/.ssh下就拷贝过jenkins路径下（不确定这一步的作用是否对结果管用）        
4）对文件授权
chown jenkins:jenkins  id_rsa   id_rsa.pub

5)重启jenkins  
/etc/init.d/jenkins restart

如果没设置管理员密码，需要通过命令获取cat   /var/lib/jenkins/secrets/initialAdminPassword

###### 设置jenkins无需登录
链接：https://www.jianshu.com/p/6591c9bdcca6

- 进入Jenkins根目录，复制config.xml 为 config.xml.bak用于备份  
- 而后打开 config.xml 配置文件；    
- 修改“< useSecurity >true< /useSecurity >”为“< useSecurity>false< /useSecurity>”；      
- 删掉“< authorizationStrategy ...>...< /authorizationStrategy>”；      
- 保存修改的confi.xml，重启Jenkins服务已无需登录 

###### 【问题1】打不开log.html 提示Opening Robot Framework log failed
Verify that you have JavaScript enabled in your browser.
Make sure you are using a modern enough browser. Firefox 3.5, IE 8, or equivalent is required, newer browsers are recommended.
Check are there messages in your browser's JavaScript error log. Please report the problem if you suspect you have encountered a bug.
..."

解决办法：
    For resolve your problem you must :
    - Connect on your jenkins url (http://[IP]:8080/)
    - Click on Manage Jenkins from left side panel.
    - Click on Script Console
    - Copy this into the field
    System.setProperty("hudson.model.DirectoryBrowserSupport.CSP","sandbox allow-scripts; default-src 'none'; img-src 'self' data: ; style-src 'self' 'unsafe-inline' data: ; script-src 'self' 'unsafe-inline' 'unsafe-eval' ;")

    - Click on Run button.
    - Execute your Jenkins build.

###### 【问题2】设置multijob构建任务时只执行第一个任务	
   忽略执行错误，继续下一个
    
   ![upload successful](/images/pasted-110.png)