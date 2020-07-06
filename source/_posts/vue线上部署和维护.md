title: vue线上部署和维护
author: 躲不掉的风
date: 2020-06-09 11:55:38
tags:
---

###### 二、数据传输

	简单办法：启动本地服务：

      python3 -m http.server 8000（python3）
      curl -O  http://10.12.169.57:8000/dist.tar.gz

####CSS文件不生效问题：
解决：1.  设置一些配置信息？-----依旧未生效
https://www.jb51.net/article/174089.htm
###### 三、 域名配置
    netstat -nltp 查看

    切换至root  :sudo su root

###### 四、 vue项目

1. 引用的css样式文件大部分没有生效？

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

2. 传文件使用 gitlab一致报错
```
Warning: Permanently added 'gitlab.com,172.65.251.78' (ECDSA) to the list of known hosts.
Permission denied (publickey).
```
ping一下gitlab.com获取ip,然后设置host对应gitlab.com

3. 上线步骤：

        更改google认证，改成线上id
        本地和git上：清除dist文件
        npm run build pro
        生成dist文件拷贝到git文件(同步使用)
        git add .
        git commit -m "update"
        git push
        服务端git pull

4. 浏览器缓存问题
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
 
 具体hash:
 
 
###### 五、google认证
1. 开发者平台：https://console.developers.google.com/apis/credentials?project=probable-goods-278213&supportedpurview=project
  	
   在以上google开发者平台注册Client ID。
    
   分别在Authorized JavaScript origins 和Authorized redirect URIs 处注册
    
   ```
    https://localhost:8080//本地
    https://test.test.io //线上
  ```
2. 利用其写登陆功能：	
   
  https://www.npmjs.com/package/vue-google-signin-button
    
具体步骤：

    1. npm install vue-google-signin-button
    
    2. 添加
    <script src="static/utils/googleclient.js"></script>
    ...
    import GSignInButton from 'vue-google-signin-button'
    Vue.use(GSignInButton)
    
    3. 登陆页
        <template>
      <g-signin-button
        :params="googleSignInParams"
        @success="onSignInSuccess"
        @error="onSignInError">
        Sign in with Google
      </g-signin-button>
    </template>

    <script>
    export default {
      data () {
        return {
          /**
           * The Auth2 parameters, as seen on
           * https://developers.google.com/identity/sign-in/web/reference#gapiauth2initparams.
           * As the very least, a valid client_id must present.
           * @type {Object} 
           */
          googleSignInParams: {
            client_id: 'YOUR_APP_CLIENT_ID.apps.googleusercontent.com'
          }
        }
      },
      methods: {
        onSignInSuccess (googleUser) {
          // `googleUser` is the GoogleUser object that represents the just-signed-in user.
          // See https://developers.google.com/identity/sign-in/web/reference#users
          const profile = googleUser.getBasicProfile() // etc etc
        },
        onSignInError (error) {
          // `error` contains any error occurred.
          console.log('OH NOES', error)
        }
      }
    }
       </script> 
        <style>
    .g-signin-button {
      /* This is where you control how the button looks. Be creative! */
      display: inline-block;
      padding: 4px 8px;
      border-radius: 3px;
      background-color: #3c82f7;
      color: #fff;
      box-shadow: 0 3px 0 #0f69ff;
    }
    </style> 

1. 打包后js文件 
         app.js：基本就是你实际编写的那个app.vue(.vue或.js?),没这个页面跑不起来.

        vendor.js:vue-cli全家桶默认配置里面这个chunk就是将所有从node_modules/里require(import)的依赖都打包到这里，所以这个就是所有node_modules/下的被require(import)的js文件

        manifest.js: 最后一个chunk，被注入了webpackJsonp的定义及异步加载相关的定义(webpack调用CommonsChunkPlugin处理后模块管理的核心,因为是核心,所以要第一个进行加载,不然会报错).

        作者：_双眸
        链接：https://www.jianshu.com/p/7a888571522d
        来源：简书
        著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
###### 六、解决问题思路

  关键block问题要学会问人，不要怕丢人
  
###### 七、 http和https实际上是跨域问题！！！
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