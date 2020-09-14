title: vue线上部署和维护
author: 躲不掉的风
date: 2020-06-09 11:55:38
tags:
---
######  同步上线文件http.server

	简单办法：启动本地服务：

      python3 -m http.server 8000（python3）
      curl -O  http://10.12.169.57:8000/dist.tar.gz

###### CSS文件不生效问题：
解决：1.  设置一些配置信息？-----依旧未生效
https://www.jb51.net/article/174089.htm
######  域名配置
    netstat -nltp 查看

    切换至root  :sudo su root

######  引用的css样式文件大部分没有生效？

  【解决】
  1.  一共三种引用方式，在main.js\index.html\app.vue中引入。  
  其中app中引入:
  ```
    <style>
      @import './../static/css/global.css'; /*引入公共样式*/
    </style>
```

	参考： https://blog.csdn.net/HY_358116732/article/details/82219387
  2. 将element-ui的引用提到最最前面
  
  因为引用的css样式是针对此ui进行的覆盖修改，所以需要在其后面引用，才起到覆盖的作用。

###### 传文件使用 gitlab一致报错
```
Warning: Permanently added 'gitlab.com,172.65.251.78' (ECDSA) to the list of known hosts.
Permission denied (publickey).
```
ping一下gitlab.com获取ip,然后设置host对应gitlab.com

###### 上线步骤：

        更改google认证，改成线上id
        本地和git上：清除dist文件
        npm run build pro
        生成dist文件拷贝到git文件(同步使用)
        git add .
        git commit -m "update"
        git push
        服务端git pull

###### 浏览器缓存问题
  - (1) nginx配置修改
  ```
  add_header Cache-Control "no-cache, no-store";
  add_header Pragma no-cache;//http 1.0旧版本
  ```
  	no-cache, no-store选择一个即可
  
  	no-cache浏览器会缓存，但刷新页面或者重新打开时 会请求服务器，服务器可以响应304，如果文件有改动就会响应200
  
	no-store浏览器不缓存，刷新页面需要重新下载页面

	Nginx下关于缓存控制字段cache-control的配置说明:
https://www.cnblogs.com/kevingrace/p/10459429.html
  - (2) 修改文件 webpack .prod.conf.js,使用时间戳动态定义版本号

  ```
  const Version = new Date().getTime();
  ...
  filename: utils.assetsPath('js/[name].[chunkhash]._' + Version + '.js'),
  ...
  filename: utils.assetsPath('css/[name].[contenthash]._' + Version + '.css'),
  ```
  
 但是后来经过实践证明，只需要配置nginx即可，每次刷新即可更新。猜测
 1.  每次html不取缓存，拿到的js都是实际服务器文件
 2.  脚手架本身使用hash生成js和css文件，会根据改动更新文件名
  
###### http和https实际上是跨域问题！！！
  本地调试如果不设置会默认Https访问，但是服务端只能接收http协议，所以需要处理
  config/index.js:
  
        proxyTable: {
            '/measurement': {
                target: 'http://10.12.170.210:8000', //后端接口地址
                changeOrigin: true, //是否允许跨越
                pathRewrite: {
                    '^/measurement': '/measurement', //重写,
                }
            }
        },
和
main.js:
```
axios.defaults.baseURL = '/measurement'
```

###### vue+elementUI打包后elmentui自带图标失效问题
原文：https://blog.csdn.net/qq_37827221/article/details/100707748https://blog.csdn.net/qq_37827221/article/details/100707748
【问题】elmentui自带图标失效，本地打prod包就没有问题，一旦把相同的文件放在服务器就不对了

![upload successful](/images/pasted-124.png)
【原因】通过本地文件上传git后，再服务端拉取git代码的方式部署
 
  但是由于git冲突(具体为啥不清楚)导致拉取git文件不全，没有拉取到font文件夹内容
  
  【解决】删除文件，重新git clone到服务器端，保证代码统一
  
![upload successful](/images/pasted-125.png)

###### 生产环境排序功能失效

【原因】为了屏蔽线上log,在webpack.pro.config中新增了限制字段 
      
        new UglifyJsPlugin({
            uglifyOptions: {
                compress: {
                    warnings: false,
                    // drop_debugger: true, //新增
                    // drop_console: true, //新增
                }
            },
本身这个没有问题，但是排序的部分使用了log打印的方式
```
console.log(val.sort(this.$compareInGroup(this.sortkey, 0, this.order)));

```
【解决】去掉console.log即可
