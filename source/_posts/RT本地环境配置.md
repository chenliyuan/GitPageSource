title: RT本地环境配置
author: 躲不掉的风
date: 2020-01-29 21:16:20
tags:
---
本地环境:Mac

- 安装谷歌浏览器

- 安装chromedriver
http://chromedriver.chromium.org/downloads
下载的driver的版本需要和浏览器的版本匹配，解压后放到本机,将文件路径添加至PATH中

- 安装本地运行环境依赖库

  如Python版本：python3.6

  requirements.txt:

      requests==2.22.0
      robotframework==3.1.2
      robotframework-ride==1.7.3.1
      robotframework-seleniumlibrary==3.3.1
      selenium==3.141.0
      xlrd==1.2.0
      Pillow==6.2.1

- 安装插件 IntelliBot 、IntelliBot @SeleniumLibrary Patched、Robot Framework support 

- Preferences-Editor-File Types-Robot Feature下添加*.robot类型

- Tools-External Tools 配置Tool Setting，其中Arguments配置如下:

  ```
  --loglevel TRACE -d /Users/liyuan/Documents/Log     $FileName$

  -d /Users/liyuan/Documents/Log --loglevel TRACE -t "$SelectedText$" ./	

  ```

 ![upload successful](/images/pasted-77.png)

 ![upload successful](/images/pasted-78.png)