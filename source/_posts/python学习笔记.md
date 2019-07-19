title: python3 HTTP请求
author: 躲不掉的风
date: 2019-06-25 17:29:18
tags:
---
利用python作HTTP请求，主要分为以下两种方法

1. ** 使用urllib  **

  在Python 3以后的版本中，urllib2这个模块已经不单独存在（也就是说当你import urllib2时，系统提示你没这个模块），urllib2被合并到了urllib中，叫做urllib.request 和 urllib.error 。

  urllib整个模块分为urllib.request, urllib.parse, urllib.error。

  例： 
  其中urllib2.urlopen()变成了urllib.request.urlopen() 
  urllib2.Request()变成了urllib.request.Request()  
  官网地址：  
  https://docs.python.org/3.5/library/urllib.html
 
 
  ```     
  # coding:utf-8
  from urllib import request
  from urllib import parse


  url = "http://10.1.2.151/ctower-mall-c/sys/login/login.do"
  data = {"id":"wdb","pwd":"wdb"}

  params="?"
  for key in data:
      params = params + key + "=" + data[key] + "&"
  print("Get方法参数："+params)

  headers = {
      #heard部分直接通过chrome部分request header部分
      'Accept':'application/json, text/plain, */*',
      'Accept-Encoding':'gzip, deflate',
      'Accept-Language':'zh-CN,zh;q=0.8',
      'Connection':'keep-alive',
      'Content-Length':'14', #get方式提交的数据长度，如果是post方式，转成get方式：【id=wdb&pwd=wdb】
      'Content-Type':'application/x-www-form-urlencoded',
      'Referer':'http://10.1.2.151/',
      'User-Agent':'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.23 Mobile Safari/537.36'

  }

  data = parse.urlencode(data).encode('utf-8')
  req = request.Request(url, headers=headers, data=data)  #POST方法
  #req = request.Request(url+params)  # GET方法
  page = request.urlopen(req).read()
  page = page.decode('utf-8')


  ```
  【报错】  
  python3 raise HTTPError(req.full_url, code, msg, hdrs, fp) urllib.error.HTTPError: HTTP Error 403: Forbid
  1.分析:

  如果用 urllib.request.urlopen 方式打开一个URL,服务器端只会收到一个单纯的对于该页面访问的请求,但是服务器并不知道发送这个请求使用的浏览器,操作系统,硬件平台等信息,而缺失这些信息的请求往往都是非正常的访问,例如爬虫.
  有些网站验证请求信息中的UserAgent(它的信息包括硬件平台、系统软件、应用软件和用户个人偏好),如果UserAgent存在异常或者是不存在,那么这次请求将会被拒绝(如上错误信息所示)
  所以可以尝试在请求中加入UserAgent的信息
  方案:
  对于Python 3.x来说,在请求中添加UserAgent的信息非常简单,代码如下:
  可以在请求加上头信息，伪装成浏览器访问User-Agent,具体的信息可以通过火狐的FireBug插件查询  
  ```
  headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:23.0) Gecko/20100101 Firefox/23.0'}  
  req = request.Request(url=chaper_url, headers=headers)  
  page  = request.urlopen(req).read() 
  ```
   但是没有解决问题，我用的python3.4不知道是不是不支持，于是改用requests
  
2.  urllib.quote  

参考：https://www.cnblogs.com/lyxdw/p/9935137.html
最近在抓请求有个这样的get参数：
```
%22pageIndex%22:0,%22pageSize%22:50,%22preference%22:%221563432582190%22,%22from%22:0%7D
```
本来以为用urllib的urlencode结果死活不行，最后发现应该直接用quote。   
再观察得知其中:/,这几个特殊字符没有转，所以safe中需要忽略处理。

```
param='{"condition":{"servicename":"/hotel/someinterface_POST"},"startTime":1563431682190,"endTime":1563432582190,"pageIndex":0,"pageSize":50,"preference":"1563432582190","from":0})'
param=parse.quote(param.encode('utf-8'),,safe=':/,')
print (param)

```
quote 和 urlencode 的区别:urlencode 需要用字典  quote单个字符就行了，因为quote只需要字符串就行了    

quote和quote_plus的区别：前者默认safe='/'；后者会将“空格”转换成“加号”，默认safe为空。

2. requests模块 

  参考
  https://blog.csdn.net/xuezhangjun0121/article/details/82024075
  因为传入的参数是以json形式传入所以headers={'Content-Type':'application/json'}
  ```
      url="http://127.0.0.1:5555/member/grade/query"
      r=requests.post(url,headers={'Content-Type':'application/json'},data=json.dumps(params))
      print (r.text)

  ```
或

  ```
  >>> payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
  >>> r = requests.get('http://httpbin.org/get', params=payload)
  >>> print(r.url)
  http://httpbin.org/get?key1=value1&key2=value2&key2=value3#已被正确编码
  ```
返回：   
```
    r.encoding                       #获取当前的编码
    r.encoding = 'utf-8'             #设置编码
    r.text                           #以encoding解析返回内容。字符串方式的响应体，会自动根据响应头部的字符编码进行解码。
    r.content                        #以字节形式（二进制）返回。字节方式的响应体，会自动为你解码 gzip 和 deflate 压缩。

    r.headers                        #以字典对象存储服务器响应头，但是这个字典比较特殊，字典键不区分大小写，若键不存在则返回None

    r.status_code                     #响应状态码
    r.raw                             #返回原始响应体，也就是 urllib 的 response 对象，使用 r.raw.read()   
    r.ok                              # 查看r.ok的布尔值便可以知道是否登陆成功
     #*特殊方法*#
    r.json()                         #Requests中内置的JSON解码器，以json形式返回,前提返回的内容确保是json格式的，不然解析出错会抛异常
    r.raise_for_status()             #失败请求(非200响应)抛出异常
  ```
待总结：https://2.python-requests.org//zh_CN/latest/user/quickstart.html