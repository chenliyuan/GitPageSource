title: Vue+Django跨域问题
author: 躲不掉的风
date: 2020-09-11 18:42:29
tags:
---
##### Django跨域问题解决

由于请求本地服务另外端口号导致的跨域问题，可参考
https://www.cnblogs.com/wasbg/p/10973880.html
https://segmentfault.com/a/1190000017952184?utm_source=tag-newest：

###### 前端解决：
	config/index.js(前提去掉Axios.defaults.baseURL)
        proxyTable: {
            '/': {
                target: 'http://127.0.0.1:8000/', //后端接口地址
                changeOrigin: true, //是否允许跨越
                pathRewrite: {
                    '^/': '/', //重写,
                }
            }
        },
###### 后端解决

 	pip install django-cors-headers   

  	setting中设置：

       1. 'corsheaders.middleware.CorsMiddleware' # 新加的插件位置最好放在CommonMiddleware之前
       
       2. INSTALLED_APPS中添加
       'corsheaders',    
       3. setting文件里新增
       
       CORS_ORIGIN_ALLOW_ALL = True   //新增   
       CORS_ALLOW_CREDENTIALS = True  //新增
       
       
 1. 设置了axios.defaults.baseURL会跳过代理
 2. 浏览器是禁止跨域的，会检查origin是否同源,若非同源则禁止。
 
 	但是服务端不禁止，在本地运行npm run dev等命令时实际上是用node运行了一个服务器，因此proxyTable实际上是将请求发给自己的服务器，再由服务器转发给后台服务器，做了亦曾代理，因为不会出现跨域问题。