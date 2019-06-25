title: TensorFlow实践
date: 2019-06-15 18:19:23
---
1. 创建隔离的python环境

```
  pip install virtualenv #安装
  virtualenv my_env#创建目录
  which python3  #找到python3目录
  virtualenv my_env -p /Library/Frameworks/Python.framework/Versions/3.7/bin/python3 
  #将python3作为解释器
  source my_env/bin/activate  #激活，开始使用
  deactivate 退出
```


