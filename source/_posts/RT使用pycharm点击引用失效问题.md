title: RT使用pycharm点击引用失效问题
author: 躲不掉的风
date: 2020-01-29 19:09:50
tags:
---
##### pycharm引用库文件失效问题

点击引用的库文件应用链接无效，导致输入相应库方法无提示，有以下几种检查办法：

- 1) 检查以下插件是否已安装

	IntelliBot、IntelliBot@SeleniumLibrary Patched 、Robot Framework support

- 2) 检查IDE设置

	如pycharm检查Project Interpreter编译器是否错选，或者没选

- 3) 检查库文件引用是否重复

如通过集成已经继承了的库文件，就不要再引用一遍了

- 4) 更换intelliBot插件

  出错也可能是因为安装的插件有问题，下载地址：https://github.com/youwi/intellibot/blob/master/intellibot.jar
  https://blog.csdn.net/qq_33783424/article/details/80783092

- 5) 检查代码格式

  Keywords 前最好留有空格；

  本次未提交文件提交上去；

- 6) 清零

	如果以上都无法解决，删除本地代码，重新拉取； 重启电脑试试