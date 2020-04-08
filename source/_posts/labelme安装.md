title: labelme安装和使用
author: 躲不掉的风
date: 2020-03-29 21:40:51
tags:
---
windows下安装和使用labelme 

 参考：https://www.jb51.net/article/166528.htm
 
  其中anaconda下载使用这个网址，(在官网下载快完成的时候停止了浪费了大把时间...)

https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

#### 步骤
1、管理员身份打开 anaconda prompt

2、输入命令：conda create --name=labelme python=3.6

3、输入命令：activate labelme

(若环境未成功激活，则改用本地cmd)

4、输入命令：pip install pyqt5，pip install pyside2(自己刚开始没有安装pyside2,运行 \anaconda安装目录\envs\labelme\Scripts\label_json_to_dataset.exe 会出现module "pyside"缺失错误)

5、输入命令：pip install labelme（由于网络原因或者库的地址，经常运行一半出现错误，不要气馁，多执行几次）

6、输入命令：labelme 即可打开labelme。如下：


7、如果将单个json文件生成数据集，运行命令：

\anaconda安装目录\envs\labelme\Scripts\label_json_to_dataset.exe    \保存xx.json文件地址

xx.json 会成生成五个文件 img.png，info.yaml，label.png，label_names，label_viz.png。其中label.png即是我们要的label_data了。

8、批量将json文件生成数据集。编写脚本如下：               

    import os
    path = 'D:\\paper\\data\\'  # path是你存放json的路径
    json_file = os.listdir(path)
    for file in json_file:
        os.system("python C:\\Users\\Monica\\.conda\\envs\\labelme\\Scripts\\labelme_json_to_dataset.exe %s"%(path + file))



![upload successful](\images\pasted-102.png)