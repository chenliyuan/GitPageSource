title: python json模块
date: 2019-07-19 10:20:35
---
1. json.dumps[卸载掉对象]   

	把一个Python对象编码转换成--->json**字符串**
2. json.loads[装上对象]    

	把json字符串解码转换成--->python**对象dict**

其中 json 有下面三种样式：
	
	字典样式 '{"name":"gzj", "age":"23", "sex":"man"}'
	列表样式 '["gzj", 23, "man"]'
	字典和列表相互嵌套的样式 """["gzj", "{'age':'23'}"]""" 
	特别注意 JSON 字符串中的内容用双引号，而非单引号。

3. json.dump和json.load均是针对文件的。
json.dump
我理解为两个动作，一个动作是将”obj“转换为JSON格式的字符串，还有一个动作是将字符串写入到文件中

	```
	a = {"name":"Tom", "age":23}
	with open("test.json", "w", encoding='utf-8') as f:
	  ... # Parameter sort_keys    是否按照字母排序
    ... # Parameter indent       缩进的空格数
    ... # Parameter separators   分割符号形式
	     json.dump(a,f,indent=4) 
	   # indent 超级好用，格式化保存字典，默认为None，小于0为零个空格
	   # f.write(json.dumps(a, indent=4))
	   # 和上面的效果一样！！！
	```
	json.load
	一个动作是将一个包含JSON格式数据从文件中取出来，还有一个动作是将其序列化为一个python对象dict
	
	**当打开文件的时候，注意加上encoding否则会因为中文报错！**
	```
	import json
	with open("test.json", "r", encoding='utf-8') as f:
	    aa = json.loads(f.read())
	    f.seek(0)
	    bb = json.load(f)    # 与 json.loads(f.read())
	```