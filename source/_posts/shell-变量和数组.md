title: shell 变量和数组
author: 躲不掉的风
date: 2020-02-06 20:17:32
tags:
---
#### 变量
  - 查看变量: echo $变量名 set(所有变量:包括自定义变量和环境变量) 
  - 取消变量: unset 变量名
  
  - 位置变量：如通过$1 $2获取第一第二个参数
  
  - 环境变量：export back_dir=xxxx  定义全局变量；或者export $var 转化成全局变量
  
  -  预定义变量
  
            $0 脚本名
            $* 所有的参数
            $@ 所有的参数
            $# 参数的个数
            $$ 当前进程的 PID
            $! 上一个后台进程的 PID
            $? 上一个命令的返回值 0 表示成功
            
     $@和$星区别：不加引号时,二者都是返回传入的参数,但加了引号后（"$@"）,此时$*把参数作为一个字符串整体(单字符串)返回,$@把每个参数作为一个字符串返回
      
  - 键盘输入变量：通过read -n 2 -p "please input: " p


  - 调用其他脚本变量值：可在b脚本中通过调用.a.sh,直接获取a的变量数据
  - “”双引号-强引用会替换里面变量值；''-弱引用，原样输出引用符号
  
  - 变量删除（获取部分字符串）
  
        ${url#*a}  从头到尾最短匹配删除即直到第一个a之前的都删除
        ${url##*a}  从头到尾贪婪匹配，删除直到最后一个匹配a
        ${url%a*}  后面加%，从后到前最短匹配删除a后面所有字符	
        ${url%%}  后面加%，从后到前贪婪匹配删除最后一个a后面所有字符
    
  - 变量替换(最短替换/和贪婪替换//)

        liyuan$ var=tom.ton.com
        liyuan$ echo ${var/to/TO}
        TOm.ton.com
        liyuan$ echo ${var//to/TO}


- 变量替代；可理解为变量初始化

      echo ${var1-aaaaa}
      echo ${var1:-aaaaa}

	使用 <font color=red>-</font> ,若没有被赋值过，就使用默认值替代(是否unset)
    
    使用 <font color=red>:-</font>，区别在于即使赋值空值也会被新值替代(是否为空值)
  
- 切片操作  echo ${url:5:7}
 
- 可使用反引号或者$()执行内部命令
  
- date +%F  格式化日期输出
- 特殊符号总结

      （） 子shell执行
      (()) 数值比较
      $()  命令替换，同反引号
      $(()) 整数运算
      
      {}  集合
      ${}  变量
      
      []  条件测试
      [[]] 条件测试，支持正则 =~
      $[]  整数运算
      
#### 数组

1. 普通数组
		一次赋多值，可指定索引
		# array2=(tom jack alice [20]=aa)
        # echo ${!array2[@]}
        0 1 2 20
    
2. 关联数组

	必须申明关联数组变量
    
          # declare -A ass_array1
          # ass_array2=([index1]=tom [index2]=jack [index4]='bash shell')
  
          查看数组
          # declare -A
          
3. 访问数组元数:
 
          # echo ${ass_array2[index2]} 访问数组中的第二个元数
          # echo ${ass_array2[@]} 访问数组中所有元数 等同于 echo ${array1[*]} 
          # echo ${#ass_array2[@]} 获得数组元数的个数
          # echo ${!ass_array2[@]} 获得数组元数的索引
          
4. 遍历数组

 通过获取数组索引遍历   