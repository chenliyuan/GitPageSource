title: Vue语法小记
author: 躲不掉的风
date: 2020-07-09 17:39:56
tags:
---
- 命名规范：

  项目文件夹命名都是 kebab-case命名
  
  属于components文件夹下的子文件夹，使用大写字母开头的PascalBase风格
  
    1. 全局通用的组件放在 /src/components下
    2. 其他业务页面中的组件，放在各自页面下的 ./components文件夹下
    3. 每个components文件夹下最多只有一层文件夹，且文件夹名称为组件的名称，文件夹下必须有index.vue或 index.js，其他.vue文件统一大写开头（Pascal case），components下的子文件夹名称统一大写开头（PascalCase）


- event和method的区别?

  method，可以在程序中直接调用对象的方法, obj.method();

  event 是在事件发生时的处理函数，通过监听事件的方式来设置
  
-  路由:使用router-link :to 参数值使用this.xxx传参失败。最后使用Push操作解决

    **和name配对的是params，和path配对的是query**
    
    query和params的区别，query相当于get请求，在页面跳转的时候，可以在地址栏看到请求参数，然而params则相当于post请求，参数不会在地址栏中显示。
    
    https://www.cnblogs.com/liweiz/p/10519629.html
        1.命名路由搭配params，刷新页面参数会丢失
        2.查询参数搭配query，刷新页面数据不会丢失
        3.接受参数使用this.$route后面就是搭配路由的名称就能获取到参数的值(如this.$route.query.xxx)
        
- main.js和routers文件重复import所有组件

	封装一个文件调用
  
  https://blog.csdn.net/yang295242361/article/details/104960383/
  
- 关于VUE中 import 、 export 和 export default

	https://www.cnblogs.com/Ryan777/p/10769661.html
    
-  bus总线传值

        bus.$emit('Click', {
            id: id,
            num:num
            })
            
        this.$EventBus.$on('uploading', val=>{
            console.log(val)       //打印出来 val ：sure
         })