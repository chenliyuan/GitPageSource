title: Vue语法小记
author: 躲不掉的风
date: 2020-07-09 17:39:56
tags:
---
1. event和method的区别?

  method，可以在程序中直接调用对象的方法, obj.method();

  event 是在事件发生时的处理函数，通过监听事件的方式来设置
  
2. 路由:使用router-link :to 参数值使用this.xxx传参失败。最后使用Push操作解决

    **和name配对的是params，和path配对的是query**
    
    query和params的区别，query相当于get请求，在页面跳转的时候，可以在地址栏看到请求参数，然而params则相当于post请求，参数不会在地址栏中显示。
    
    https://www.cnblogs.com/liweiz/p/10519629.html
        1.命名路由搭配params，刷新页面参数会丢失
        2.查询参数搭配query，刷新页面数据不会丢失
        3.接受参数使用this.$route后面就是搭配路由的名称就能获取到参数的值(如this.$route.query.xxx)