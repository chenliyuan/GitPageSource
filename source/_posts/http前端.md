title: http前端
author: 躲不掉的风
date: 2019-10-26 15:25:31
tags:
---
##### http版本：

    http1.0:需要使用keep-alive建立长连接（因为创建tcp连接经过三次握手有一定的开销）。从1.1开始默认支持长连接。
    http1.1:支持只发送header、断点续传、host域
    http2.0（基于https）:多路复用、header数据压缩、服务器推送
    http强制缓存：只要不过期就要走缓存，缺点是不能及时更新。
    http协商缓存：没有修改返304状态码，否则200响应结果。
##### 浏览器输入网址到加载完页面发生了什么？
    1、	查找DNS缓存（浏览器缓存、操作系统缓存、host查看域名IP、运营商的DNS服务器缓存）
    2、	DNS域名解析，查找相应IP（过程是从后面往前递归查找）如com->baidu->www
    3、	发起TCP3次握手 建立TCP连接
    4、	发送http请求  
    5、	服务端处理请求    MVC分层
    6、	浏览器接收响应，解析html文件请求静态资源（css/js/imge，若服务器返回304说明请求资源没做修改从本地缓存取即可）
    7、	根据http响应渲染前端页面，构建DOM树
    8、	关闭tcp连接（四次挥手手）
参考：https://www.cnblogs.com/zgq123456/articles/11641187.html

【几个问题】：

    1、http新版本请求除非connection:close时会断开TCP，否则在请求完成之前不会断开
    2、一个TCP连接中可以对应几个http请求？多个
    3、一个TCP连接中可以一起发送多个http请求么？
    在http2.0提供了多路复用（Multiplexing）支持此功能。
    4、为什么有的时候刷新页面不需要重新建立SSL连接
     因为TCP连接维持的时候，SSL自然也会用以前的
    5、浏览器对同一HOST建议TCP连接到数量有没有限制？
    有 chrome最多建议6个TCP连接。不同浏览器限制不同。
    
##### 请求报文的格式
请求行：url、http协议版本、请求方式
请求头：
Accept
Accept-language
Accept-Encoding
Cache-Control:
Cookie
Connection:
Content-Type:
Content-Length:
Host:
Referer:
User-Agent:

请求体：数据正文(入参)
#### 请求方法：
  GET POST DELETE PUT HEAD
  
    DELETE:请求服务端删除指定页面
    PUT：使传入数据取代指定的文档内容
    HEAD：类似GET，只是返回内容里只有报头
##### 状态码：
    1xx：标识请求已接受，继续处理
    2xx：成功
    3xx：重定向
    4xx：客户端错误
    5xx：服务端错误
    常用几个：
    302:重定向，比如把http请求都转成https
    400：客户端语法错误
    403：服务器拒绝提供服务
    404：请求资源不存在
    500：服务器内部错误
#### cookie和session
cookie相当于用户身份证，session相当于客户表记录