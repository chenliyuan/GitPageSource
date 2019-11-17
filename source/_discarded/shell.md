title: shell
author: 躲不掉的风
date: 2019-10-09 10:16:55
tags:
---
**基础操作**
```
#!/bin/bash
name="runoob"
#单引号拼接
greeting_1='hello'${name}'!'
#双引号拼接
greeting_2="hello,${name}"
echo $greeting_1 $greeting_2
len=${#name}
echo 'name长度是 '$len
echo '切片2-4'${name:1:3}
echo"文件名是$0"
echo'第一个参数是'$1
echo '参数个数 '$#
####数组
my_array=(A B "c" d)
echo "数组的第一个元素为 ${my_array[0]}"
echo '数组的第三个元素为 '${my_array[2]}
echo '数组个数'${#my_array[@]}
####
printf "Hello, Shell\n"
#for循环
for item in 1 2 3 4 5
do 
    echo "the item is ${item}"
done
#while循环
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```