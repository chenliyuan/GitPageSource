title: MAC配置安卓环境sdk
author: 躲不掉的风
date: 2020-09-14 20:37:11
tags:
---
1. 打开官网-Android SDK工具--SDK Tools下载对应sdk  zip包
2. 下载后解压
3. 打开终端，输入

		vim ~/.bash_profile        
4. 添加如下
 
	export ANDROID_HOME=/Users/shmaur/android-sdk-macosx  //这里为 sdk 路径地址
    export PATH=$PATH:$ANDROID_HOME/tools
    export PATH=$PATH:$ANDROID_HOME/platform-tools
    
5. 每次编辑完 .bash_profile 文件后必须要进行：

	   source .bash_profile //更新环境变量配置，不更新则不会使配置生效
    
6. 配置好后可以查看一下是否成功：

		echo $PATH