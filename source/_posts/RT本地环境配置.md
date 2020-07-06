title: Robot Framework环境配置和部署
author: 躲不掉的风
date: 2020-01-29 21:16:20
tags:
---
##### 安装谷歌浏览器

	在win上安装：现在百度找个可用的chrome包都很难，最后干脆下载了个软件管理，瞬间安装好了。。。

	不过要想找历史版本就得海淘碰运气了，本来下载程序依赖79，但是后来79貌似不行了，一直报错，一段时间后还是又升级回了最新80版本
    
- 安装chromedriver

  http://chromedriver.chromium.org/downloads
  或
  http://chromedriver.storage.googleapis.com/index.html

  下载的driver的版本需要和浏览器的版本匹配，解压后放到本机,将文件路径添加至PATH中

    1. 将下载的可执行文件移到/usr/local/bin下

    sudo mv chromedriver /usr/local/bin/chromedriver

    2. : 修改文件权限

    sudo chmod u+x,o+x /usr/local/bin/chromedriver

    3. 检查是否安装成功:

        chromedriver --version

##### 安装本地运行环境依赖库

  如Python版本：python3.6

      requests==2.22.0
      robotframework==3.1.2
      robotframework-ride==1.7.3.1
      robotframework-seleniumlibrary==3.3.1
      selenium==3.141.0
      xlrd==1.2.0
      Pillow==6.2.1
      
      快速生成requirement.txt的安装文件
      pip freeze > requirements.txt
      安装所需要的文件
      pip install -r requirement.txt

##### 安装配置IDE--推荐pycharm
- 安装插件 IntelliBot 、IntelliBot @SeleniumLibrary Patched、Robot Framework support 

- Preferences-Editor-File Types-Robot Feature下添加*.robot类型

- Tools-External Tools 配置Tool Setting，其中Arguments配置如下:

  ```
  --loglevel TRACE -d /Users/liyuan/Documents/Log     $FileName$

  -d /Users/liyuan/Documents/Log --loglevel TRACE -t "$SelectedText$" ./	

  ```

 ![upload successful](/images/pasted-77.png)

 ![upload successful](/images/pasted-78.png)
 
##### 运行Case

使用`robot`命令，jenkins可结合`rebot`使用

    robot --output original.xml tests                          # first execute all tests
    robot --rerunfailed original.xml --output rerun.xml tests  # then re-execute failing
    rebot --merge original.xml rerun.xml                       # finally merge results