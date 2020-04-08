title: Anaconda  conda使用
author: 躲不掉的风
date: 2020-03-18 18:28:58
tags:
---
##### 1、安装

  -  环境 mac ：官网下载sh包，

      sh   Miniconda3-latest-MacOSX-x86_64.sh

  安装后去/xxx/miniconda3/bin下执行 
  
      .  activate
  -  windows安装
  
 	下载： https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
    
#### 2、conda命令

        conda #可查帮助信息
        conda info -e
        source activate deeplearning   #进入我们的虚拟环境其中deeplearning是名称
          #退出虚拟环境
        conda remove -n deeplearning --all
	    conda list 查看已安装软件
        conda available packages
        conda search source deactivategatk
        conda install gatk
 		which gatk  查看软件安装位置
        conda update gatk  升级软件
        conda remove gatk  卸载指定软件
        conda env list  环境列表，星号代表当前环境
        activate learn  切换到learn环境
        activate // 切换到base环境
        conda create -n learn python=3 // 创建一个名为learn的环境并指定python版本为3(的最新版本)
        conda remove -n learn --all // 删除learn环境及下属所有包


  虚拟环境的使用：

  ![upload successful](/images/pasted-93.png)

#### 3、问题
- 【Anaconda】关于conda使用环境未被激活的问题

		由powershell改成使用cmd
    
	参考：https://www.cnblogs.com/happyamyhope/p/11547134.html