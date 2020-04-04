title: shell命令
author: 躲不掉的风
date: 2019-10-23 17:18:30
tags:
---
#### shell执行
- 执行脚本
	
      ./01.sh   需要执行权限  在子shell中执行
      bash 01.sh   不需要执行权限  在子shell中执行

      . 01.sh  不需要执行权限  在当前shell中执行
      source 01.sh 不需要执行权限  在当前shell中执行
    
- 调试脚本
    
      sh -n 02.sh   验证语法是否有错
      sh -vx 02.sh  查看运行详情
    
	若想让代码影响当前shell需要在主shell中执行
    
- bg 后台 ，fg 前台

  例如：执行 vi /etc/hosts可以通过ctrl+Z暂停退出，在shell窗口通过fg再返回来
- cat < /etc/hosts 同 cat  /etc/hosts
- cat <<EOF 把后面输入的内容给cat作为参数执行
- cat   >file  <<EOF 将后面输入的内容重定向到file文件中

##### 命令行排序 ；& ||

      ;  无论怎样每条都执行  
      &  上一条为真下条才执行  
      ||  上条为假下一条才执行  
     
    
#### 运算总结
使用let、expr、$(())或$[]进行基本的整数运算，使用bc进行高级的运算，包括小数运算。其中expr命令也能进行整数运算，还能判断参数是否为整数.除此外还有awk

      A=5
      B=6
      C= $[$A+$B]  # 方法 1
      let C=$A+$B     #方法2 :可使用let i++
      C= $(($A+$B)) # 方法 3
      C=`expr $A + $B`   # 方法 4
      其他：
      C=`echo $A+$B | bc` # 方法 5
      awk 'BEGIN{print '"$A"'+'"$B"'}'  # 方法 6

  
#### 其他

- shell中调用Python程序

 使用相同的标识符比如EOF，其中-为了格式考虑
      #!/usr/bin/bash
      echo "hello shell"

      /usr/bin/python <<-EOF
      print("hello python")
      EOF

- 变量使用推荐加花括号，如${name}，但定义变量时，变量名不加美元符号！

- 字符串可以用单引号，双引号，也可以不用引号
- 语句中一般不包含分号，空格/tab，以及冒号的标点符号；只在特殊条件下使用。
	- 分号：只有在代码块都写在一行的时候用分号区分，注意then后面和结束不用分号如if [ "$PS1" ]; then echo test is ok; fi
    
  - 冒号：主要用于参数扩展。如echo ${a:-newword}或者{a:=newword}即--- word前的“-”可以理解为“没定义，则替换成word”；“+”正相反，可以理解为“有定义，则替换成word”。“?”可以理解为“参数到底定义了没，没定义，把word当错误消息打印出来。”
除此获取子字符串可以用${parameter:offset}和${parameter:offset:length}若倒数，则加负号如echo $(var: -5)区别于(var:-5)，后者是没定义则替换成5
- 函数内部使用$n来调用入参

      $0当前脚本名称
      $n 参数名
      $#参数总个数
      $*以一个字符串显示所有参数，与$@同义，前者一个字符串输出，后者N个
      $?上个命令的推出状态或函数的返回值
      $$当前shell进程ID
- 和其他语言不同，调用函数仅使用函数名即可,参数直接空格跟后面，没有括号
- 和其他语言不同，shell语言中0代表true，其他值代表false

- 条件表达式要放在方括号[]之间，并且所有地方都要有空格如[ $a == $b ]
==用于数字比较[ $a == $b ] ，=用于字符串比较[ $a = $b]
条件表达式中运算符前面都有横杠-
-f 表示文件(普通)存在
条件控制：if 要加then fi 循环控制要加do done
- 引用其他文件： .filename或者source filename
- 字符串运算符：单目-z(长度为0) -n（长度不为0） $（是否为空）
- 命令export作用是使变量可以在子shell中使用，与source类似也有区别。
- 使用nohup command &使程序在后台运行，hohup作用是即使session关闭程序也不中断
- 输入重定向与输出相反，command< filename ，是将本来从键盘获取的输入变成了文件，然后利用command执行该文件
- 追加重定向都是双大于等于号，如>>或<<
- 读取文件read:使用 cat 命令并通过管道将结果直接传送给包含 read 命令的 while 命令。如cat test.txt | while read line
大括号的作用：相当于创建了个匿名函数，左括号后必须有空格，右括号前的命令必须有分号。

- 怎样获取文件第k行？ head -k filename|tail -1 其中head/tail -n k与head/tail -k相同
或者sed -n 'kp' filename
- egrep除了grep -e的功能外，还可以用扩展表达式egrep "^test|^root" /etc/hosts
  grep xxx filename  文件中包含xx的行记录

      写程序为用户计算主组数目并显示次数和组名

      cat /etc/passwd|cut -d: -f4|sort|uniq -c|while read c g
      do
      { echo $c; grep :$g: /etc/group|cut -d: -f1;}|xargs -n 2
      done
      
- 文件描述符0代表标准输入、1代表标准输出、2代表标准错误输出。
若错误输出和标准输出都同时重定向到一个位置，使用2>&1如

    	ls /usr/share/doc > out.txt 2>&1