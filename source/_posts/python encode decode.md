title: 'python encode decode '
date: 2019-07-12 14:55:45
---
这里只说python3,不谈python2

1、字符串通过编码转换为字节码，字节码通过解码转换为字符串   
 
** str（unicode）--->(encode)--->bytes，bytes--->(decode)--->str **

python3默认编码是unicode编码，非utf8,需要经过encode才可能到utf8.
```
name="小明"   #类型：<class 'str'>
name1=name.encode("utf-8")  #加密输出：b'\xe5\xb0\x8f\xe6\x98\x8e'
name2=name1.decode("utf-8") #解密还原：'小明'

```
 u'\u7b80' 是unicode, '\xe7\xae\x80'是byte , byte和unicode之间一一对应, 可以相互转换, 转换规则如下:
 ```
>>>b'\xe7\xae\x80'.decode('utf-8')   #字节转unicode（str）
'简'
>>>u'\u7b80'.encode('utf-8')    #unicode转字节
b'\xe7\xae\x80'
 ```
 参考：https://www.jianshu.com/p/4ced2bbc9334
 https://blog.csdn.net/qq_29053519/article/details/79170519