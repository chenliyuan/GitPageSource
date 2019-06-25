title: 搭建博客踩得最主要的坑
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