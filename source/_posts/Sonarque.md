title: Sonarque
author: 躲不掉的风
date: 2020-08-17 11:21:26
tags:
---
![upload successful](/images/pasted-126.png)
### 理论和安装准备
###### sonarque对js的安全检查
导致漏洞：

    1、凭据允许访问敏感组件，例如数据库，文件存储，API或服务。 
    2、标准输入
    3、使用命令行
    4、使用套接字
    5、Xpath注入    
    原因： 就是XPath解析器本身对URL、表单中提交的代码内容未做严格限制，导致恶意代码可以直接解析执行。
    举例：如
       //users/user[loginID/text()=’abc’ and password/text()=’test123’] 中
       注入：/users/user[LoginID/text()=''or 1=1 and password/text()=''or 1=1]。
    6、散列数据(哈希)
    7、加密数据
    8、正则表达式敏感  * + {
    9、跨文档消息传递
    10、随机数生成器
    11、SQL查询
    12、动态执行代码 (硬编码)
    let func = Function（'obj'+ propName）; //敏感
    13、使用OS命令时使用Shell解释器
    14、跨域资源共享
    15、Cookie
    
######  相关概念    
    
    硬编码：就是将数据直接写入到代码中进行编译开发
    软编码：则是将数据与源代码解耦
    硬编码就是什么都在你的程序代码里面写死了，你想稍微修改一下效果，都得修改你的代码。
    举个例子：举个例子，比如说你做个软件，他有菜单栏，你如果把菜单的标题全部写在代码里，那如果现在要换英文的，你就不得不改变代码。
    现在换一种方式，你把菜单标题全部写在一个文本里，比如叫title.txt，现在你要英文，那么只要把title.txt里面相对应的值换成英文就可以了。
    不用在去该代码本身。
   
######  连接数据库
 1. 安装mysql
 
 报错 org.sonar.process.MessageException: Unsupported JDBC driver provider: mysql
 
 ** SonarQube 7.9以上版本已不再支持mysql**

 2. 安装PostgreSQL 

 https://www.cnblogs.com/sea-stream/p/11197603.html

 如何配置sonar.properties

	https://www.cnblogs.com/Simple-Small/archive/2020/05/13/12882948.html    
 
 3. 通过命令扫描：
![upload successful](/images/pasted-129.png)

 4. 中文插件
 
  SonarQube默认为英文，我们可以安装SonarQube提供提供了中文插件，以便更好地熟悉使用。

  github地址：

  https://github.com/SonarQubeCommunity/sonar-l10n-zh

  找到对应的released版本jar包下载后，放入sonar目录如下后重启

  		sonarqube-6.5/extensions/plugins
        

###  实践
######  运行sonar

官网下载zip报(或者百度云)

- 解压后相关命令

		sh sonarqube-8.3.1.34397/bin/macosx-universal-64/sonar.sh  console
        
        tail -f /Users/liyuanchen/Documents/sonarqube-8.3.1.34397/logs/sonar.log
        
        打开本地9000端口即可
      
- 扫描项目

   ![upload successful](/images/pasted-130.png)
