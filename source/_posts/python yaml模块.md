title: python yaml模块
---

1. 大小写敏感
 2. 使用缩进表示层级关系
 3. 缩进时不允许使用Tab，只允许使用空格
 4. 缩进的空格数目不重要，只要相同层级的元素左对齐即可
 5. ‘#’表示注释，从它开始到行尾都被忽略

	yaml模块常用的两个方法时yaml.load和yaml.dump

1. yaml.load
 返回一个对象dict

	```
	import yaml
	f = open(r'E:\AutomaticTest\Test_Framework\config\config.yml')
	y = yaml.load(f)
	print (y)
	```

 2. yaml.dump 将一个python对象生成为yaml文档
 	若有中文，必须添加allow_unicode参数
	第二个参数，是要写进的文件
	```
	import yaml
		with open('abc.conf','w')as f:
		    yaml.dump(self.config, f, default_flow_style=False,
		                      indent=2, allow_unicode=True)
	```
参考：https://www.cnblogs.com/klb561/p/9326677.html
