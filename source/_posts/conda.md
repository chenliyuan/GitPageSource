title: conda使用
author: 躲不掉的风
date: 2020-03-18 18:28:58
tags:
---
##### 安装

  环境 mac ：官网下载sh包，

      sh   Miniconda3-latest-MacOSX-x86_64.sh

  安装后去/xxx/miniconda3/bin下执行 
  
      .  activate

##### conda命令

        conda 可查帮助信息
        conda info -e
        source activate deeplearning   进入我们的虚拟环境其中deeplearning是名称
          退出虚拟环境
        conda remove -n deeplearning --all
	    conda list 查看已安装软件
        conda available packages
        conda search source deactivategatk
        conda install gatk
 		which gatk  查看软件安装位置
        conda update gatk  升级软件
        conda remove gatk  卸载指定软件
        

虚拟环境的使用：

![upload successful](/images/pasted-93.png)

