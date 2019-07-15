title: python yaml模块
date: 2019-07-12 14:29:36
---
安装：pip install pyyaml  

**语法 **：  
 1. 大小写敏感 
 2. 使用缩进表示层级关系
 3. 缩进时不允许使用Tab，只允许使用空格
 4. 缩进的空格数目不重要，只要相同层级的元素左对齐即可
 5. ‘#’表示注释，从它开始到行尾都被忽略

	yaml模块常用的两个方法时yaml.load和yaml.dump

2. yaml.load
 返回一个对象dict

	```
	import yaml
	f = open(r'E:\AutomaticTest\Test_Framework\config\config.yml')
	y = yaml.load(f)
	print (y)
	```
3. yaml.dump 将一个python对象生成为yaml文档,除此还有safe_dump等  
 	若有中文，必须添加**allow_unicode**参数   
	第二个参数，是要写进的文件
    
   ```
        dump(data, stream=None, Dumper=Dumper,
          default_style=None,
          default_flow_style=None,
          encoding='utf-8', # encoding=None (Python 3)
          explicit_start=None,
          explicit_end=None,
          version=None,
          tags=None,
          canonical=None,
          indent=None,
          width=None,
          allow_unicode=None,
          line_break=None)
        dump_all(data, stream=None, Dumper=Dumper, ...)  

        safe_dump(data, stream=None, ...)  

        safe_dump_all(data, stream=None, ...)  

 ```
    举例：
    ```
    import yaml
    with open('abc.conf','w')as f:
       yaml.dump(self.config, f, default_flow_style=False,
       indent=2, allow_unicode=True)
    ```
    JSON转YAML
    ```
    import json,yaml
    str = '{ "foo": "bar" }'
    data = json.loads(str)
    yml = yaml.safe_dump(data,allow_unicode=True)
    print(yml)
    ```

参考：http://www.361way.com/python-pyyaml-module/4271.html
参考：https://www.cnblogs.com/klb561/p/9326677.html