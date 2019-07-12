title: 搭建博客(blog)踩得最主要的坑
author: 躲不掉的风
tags:
  - blog
categories: []
date: 2019-06-07 22:39:00
---
参考博客：https://samxrtq.github.io/2019/05/16/%E5%BB%BA%E7%AB%8BHEXO-github%E5%8D%9A%E5%AE%A2/


问题1：
hexo fs.SyncWriteStream is deprecated

查看各种博客，还让再安装一系列的包，最后一步步入坑，比如这种
```
npm install hexo-fs --save
npm install hexo-deployer-git@0.3.1 --save
npm install hexo-renderer-ejs@0.3.1 --save
npm install hexo-server@0.2.2 --save

npm audit fix

```
回头来看，其实是思路除了问题，最终从头抓起，应该找到问题源头才行。最后查到了一篇对路解决方案
https://blog.csdn.net/qq_31975963/article/details/82882465

在hexo项目中其中有一个hexo-fs的插件调用了这个方法，所以需要更新hexo-fs插件，更新方法如下：

npm install hexo-fs –save

更新插件后问题依然无法解决。

通过–debug来查看在哪里出了问题：

通过–debug来查看：

[root@server init]# <font color=red> hexo –debug</font>   
06:55:32.711 DEBUG Hexo version: 3.5.0  
06:55:32.714 DEBUG Working directory: /data/wwwroot/init/  
06:55:32.787 DEBUG Config loaded: /data/wwwroot/init/_config.yml
06:55:32.832 DEBUG Plugin loaded: hexo-admin
(node:25414) [DEP0061] DeprecationWarning: fs.SyncWriteStream is deprecated.

问题出在：hexo-admin的hexo-fs
因hexo-admin作为后台管理，无法npm uninstall hexo-admin卸载,则找到对应文件，注释：

打开文件
./node_modules/hexo-admin/node_modules/hexo-fs/lib/fs.js

找到 718行:exports.SyncWriteStream = fs.SyncWriteStream;
将对应的exports.SyncWriteStream = fs.SyncWriteStream;注释(前面 //)即可！

------------------
在windows上同步搭建
参考：https://samxrtq.github.io/2019/05/17/Windows%E4%B8%8Emac%E5%8F%8C%E7%AB%AF%E7%94%A8hexo%E5%86%99%E5%8D%9A%E5%AE%A2/

问题：
1、如何在本机创建多个key
```
#第一步 创建别名key
ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "entere@126.com" 
#第二步创建config文件
touch ~/.ssh/config
chmod 600 ~/.ssh/config#根据需要进行这一步（win上没用这一步）
#第三步 编辑config文件，将私钥地址附上去
Host github.com
    IdentityFile ~/.ssh/id_rsa.github

```
2、hexo d -g的时候提示git不是内部命令
解决：检查环境变量，后来发现环境变量的目录git执行文件名字是git_cmd.exe,于是将其改名为git.exe.

![upload successful](\images\pasted-29.png)
3、上个问题解决后随即提示add不是内部或外部命令
解决：换成bash命令，脱离cmd

 
![upload successful](\images\pasted-30.png)
4、错误：git地址提交混乱，其中gitpagesource应该放源码，不该提到原来的git库里，后来重新按照步骤来了一遍好了。  
5、不该更改_config.yml里原来的配置。  
6、图片上传后不展示？
找大牛解决，替换了marked.js文件  
7、mac和win不同步？
大牛给贴心画出原理图后秒懂，原因是源码没有在git仓库上同步
![upload successful](\images\pasted-33.png)

1.git pull    #将源码从gitpagesource仓库更新
2.hexo s后，用admin写博客
3.hexo d -g   #将博客推送到chenliyuan.github.io  （hexo d会清空chenliyuan.github.io这个仓库，然后将发布后的html们放上去，chenliyuan.github.io这个仓库就是网址要显示的内容）
4.git add.  commit  push  #这一步是为了把源代码上传到GitpageSource仓库，以防你下一个设备用的时候，源代码不同步
5.以.md文件存储的，程序会将这些.md文件，给转换为html;你的主题设置，hexo设置，你的网站颜色设置，等等++++你写的博客源码(.md文件)+++图片.然后你hexo g的时候，hexo就会将这些源码进行编译，发布到public文件夹下，那里就是发布后的    其实是一个网站



