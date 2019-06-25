title: python3升级后不支持的2.x文件的操作
---
**python 3.3.2报错：No module named 'urllib2' 解决方法**
在python3.3里面，用urllib.request代替urllib2
 **ImportError: No module named 'ConfigParser**
from configparser import  ConfigParser#引用文件全部变成小写
**AttributeError: module 'urllib' has no attribute 'quote'**
urllib.quote改成urllib.request.quote
**POST data should be bytes or an iterable of bytes. It cannot be of type str.**
  最后通过交流发现需要加在urlencode语句后加encode(encoding='UTF8')
同时，在输出时，再使用decode("utf-8")进行解码，往往更加能够取的理想的效果。

```
 1 from urllib import request
 2 from urllib import parse
 3 
 4 url = "https://movie.douban.com/j/chart/top_list?type=11&interval_id=100%3A90&action="
 5 headers = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36"}
 6 formdata = {
 7     'start': '0',
 8     'limit': '10'
 9 }
10 
11 data = parse.urlencode(formdata).encode(encoding="utf-8")
12 req = request.Request(url=url,headers=headers)
13 response = request.urlopen(req)
14 print(response.read().decode("utf-8"))
```

--------------------- 

**TypeError: Can‘t convert ‘bytes‘ object to str implicitly**
解决方法：使用字节码的decode()方法。

示例：

	str = ‘I am string‘
	byte = b‘ I am bytes‘
	s = str + byte
	print(s)
　　这时会报错：TypeError: Can‘t convert ‘bytes‘ object to str implicitly

解决方法：

	s = str + byte.decode()
**python reload(sys)找不到，name 'reload' is not defined**
在3.x中已经被毙掉了被替换为

	import importlib
	importlib.reload(sys)
 	sys.setdefaultencoding(“utf-8”) 这种方式在3.x中被彻底遗
 **关于 urllib.request.urlopen()的错误**
 这种错不常见，主要是由于我的文件名称含有http.py，这里应该是关键字敏感导致的错误。删除这个文件的子文件和修改文件名后就可以正常运行了！