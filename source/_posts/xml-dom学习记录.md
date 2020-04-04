title: xml.dom学习记录
author: 躲不掉的风
date: 2020-03-17 20:04:06
tags:
---
参考：https://www.cnblogs.com/xiaowuyi/archive/2012/10/17/2727912.html
主要方法：

minidom.parse(filename)：加载读取XML文件

doc.documentElement：获取XML文档对象

node.getAttribute(AttributeName)：获取XML节点属性值

node.getElementsByTagName(TagName)：获取XML节点对象集合

node.childNodes ：返回子节点列表。

node.childNodes[index].nodeValue：获取XML节点值

node.firstChild：访问第一个节点，等价于pagexml.childNodes[0]



实践：逐层获取子Node的值

源文件：

![upload successful](/images/pasted-92.png)

```
#encoding=utf-8
from xml.dom.minidom import parse
import xml.dom.minidom
DOMTree = xml.dom.minidom.parse("/Users/ly/Documents/config.xml")
root = DOMTree.documentElement #得到dom对象的根节点
screenshotElementObj=root.getElementsByTagName("otherFiles")
print(screenshotElementObj)
if screenshotElementObj:
    scobj=getElementsByTagName("otherFiles")[0].getElementsByTagName("string")
    print(scobj)
    print("childNodes：",scobj[0].childNodes)
    for node in scobj[0].childNodes:
        if node.nodeType in (node.TEXT_NODE,):           	      
        	print(node.nodeName,node.nodeType,node.data)
```
运行结果：
```
[<DOM Element: otherFiles at 0x10a0a54d0>]
[<DOM Element: string at 0x10a0a55a8>]
('childNodes\xef\xbc\x9a', [<DOM Text node "u'selenium-s'...">])
('#text', 3, u'selenium-screenshot-*.png')
[Finished in 0.1s]
```
备注：
1. 获取节点后是个List，即使有一项也要记得别忘了加索引如
getElementsByTagName("otherFiles")

2. 获取到childnotes节点后可以逐个判断若是text类型就打印，另外知道nodechild列表也可以直接通过索引打印，有时候可以替换成
    print("result:",scobj[0].childNodes[0].data)
    
3.  有时候提示没有data属性，可能就是None需要判断一下


