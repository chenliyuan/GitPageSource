title: google认证和授权登陆
author: 躲不掉的风
date: 2020-07-24 14:02:01
tags:
---
###### google认证
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
        
#####  auth2 授权

有使用授权邮箱的需求，于是该用auth2


前提是已注册了clientid

官网
https://developers.google.com/identity/sign-in/web/reference
 
 VUE中实现
 1. 首先html中引用
     ```
         <script src="https://apis.google.com/js/client.js" async defer></script>
    ```
 2. init
     ```
     let that = this;
          window.gapi.load("auth2", function() {
            if (!window.gapi) {
              throw new Error(
                '"https://apis.google.com/js/api:client.js" needs to be included as a <script>.'
              );
            }
            that.auth = window.gapi.auth2.init({
              client_id:
                "386555551764-ii8s4iv83e6bev0akiquniqsl2oh64q1.apps.googleusercontent.com",
              // Scopes to request in addition to 'profile' and 'email'
              scope: "https://mail.google.com/"
            });
            console.log("auth2", that.auth);
          });
        }
      }
     ```
     
 3. 获取授权code和身份认证信息
    ``` 
   let that=this;
   var GoogleAuth = window.gapi.auth2.getAuthInstance();
      this.auth.grantOfflineAccess().then(function(authResult) {
      //获取到认证码
      console.log(authResult.code)
      //登陆后方可获取用户身份信息，之前获取为空
        var GoogleUser = GoogleAuth.currentUser.get()
        const profile = GoogleUser.getBasicProfile();
        const authResponse = GoogleUser.getAuthResponse();
    ```
   得到的code传给后端用于：http://www.zyiz.net/tech/detail-142921.html

    - code是第三方用来获取access_token的，code的超时时间为10分钟，一个code只能成功换取一次access_token即失效。code的临时性和一次性保障了微信授权登录的安全性。
    - 通过code参数加上AppID和AppSecret等，通过API换取access_token（接口调用凭证）。

    - 通过access_token进行接口调用，获取用户基本数据资源或帮助用户实现基本操

4. scope更换后登陆依旧带有原来的权限？

    https://myaccount.google.com/permissions 去除原来的权限