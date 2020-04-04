title: shell 函数
author: 躲不掉的风
date: 2020-02-06 20:07:46
tags:
---

##### 1. 定义函数
###### 方法一（常用）:

	函数名() { 函数要实现的功能代码 }
###### 方法二:
	function 函数名 { 函数要实现的功能代码 }
##### 调用函数

	函数名 函数名参数1 参数2

举例：

    #!/usr/bin/bash
    jiecheng(){
    result=1
    for i in `seq $1`
    do
            echo $i
            result=$[$result*$i]
    done
    echo $result
    }

    jiecheng $1

    1.实践证明，do中的result必须不能加$符号，否则错误
    2.$[]运算结果赋值的时候，=左右不能有空格


#### 总结
- 函数接收位置参数 $1 $2... $n

- 函数接收数组变量使用$@ $*

- 函数接收参数个数$#

- 函数将受到的所有参数赋值给数组 newarray=($*)

- 养成在函数内部的变量习惯性加local关键字供内部函数使用的习惯