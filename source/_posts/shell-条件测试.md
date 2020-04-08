title: shell 条件测试
author: 躲不掉的风
date: 2020-02-06 12:21:04
tags:
---
Shell 条件测试

    格式 1: test 条件表达式 
    格式 2: [ 条件表达式 ] 
    格式 3: [[ 条件表达式 ]]

1. 文件测试

        test -d /home
        [ -e dir|file ]  True if file exists (regardless of type)
        [ -d dir ] True if file exists and is a directory.
        [ -f file ] True if file exists and is a regular file.
        [ -d /ccc ] || mkdir /ccc
      
2. 数值比较 

        [ 整数1  操作符  整数2] 
        [ 1 -gt 10 ] 大于
        [ 1 -eq 10 ] 等于
3.  数值比较--C语言风格 

        [root@xxxx ~]# ((1<2));echo $?
        0
        [root@xxxx ~]# ((1==2));echo $?
        
4. 字符串比较

	    等号
        [root@xxxx ~]# [ "$USER" = "root" ];echo $?
        [root@xxxx ~]# [ -z "$var1" ];echo $?
        条件判断
        [root@xxxx ~]# [ 1 -lt 2 -a 5 -gt 10 ];echo $?
        [root@xxxx ~]# [ 1 -lt 2 -o 5 -gt 10 ];echo $?
    	使用正则	
        [root@yangs ~]# [[ "$num10" =~ ^[0-9]+$ ]];echo $?
    
	【注】:变量为空 或 未定义: 长度都为 0
    
5. eq  ge等用于数字比较

   = >等都是字符串比较	
  
	字符串比较一般要加双引号
    
6. 在shell脚本里，推荐按以下方式声明和使用布尔类型。

        bool=true
        if [ "$bool" = true ]; then
        if [ "$bool" = "true" ]; then
        if [[ "$bool" = true ]]; then
        if [[ "$bool" = "true" ]]; then
        if [[ "$bool" == true ]]; then
        if [[ "$bool" == "true" ]]; then
        if test "$bool" = true; then
        if test "$bool" = "true"; then
        
        
 7. 实例 判断bool变量不存在或者为false
 
 		if [ ${#usePFB} -eq 0 -o "$usePFB" = "false" ]