title: awk知识点汇总
date: 2018-08-22 15:23:35
---
参考原文：https://blog.csdn.net/liang5603/article/details/80855426

grep sed awk 被称为linux中的“三剑客”
grep更适合单纯的查找或匹配文本
sed更适合编辑匹配到的文本
awk更适合格式化文本，对文本进行较复杂格式处理
##### awk语法： 

	awk [options] 'commands' filenames
    	
    - options:-F 定义输入字段分隔符，默认的分隔符是空格或制表符(tab)
    - command:
    
		BEGIN{} {} END{}
        行处理前 行处理 行处理后
  例：      
  
    $ awk 'BEGIN{print "begin:"1/2}{print "ok"}END{print"--ending-"}' vimtest.txt 
    begin:0.5
    ok
    ok
    --ending-	

##### awk执行原理
  1. awk 使用一行作为输入，并将这一行赋给内部变量$0，每一行也可称为一个记录，以换行符结束
  - (2)然后，行被: (默认为空格或制表符)分解成字段(或域)，每个字段存储在已编号的变量中，从$1 开始，最多达 100 个字段
  - (3)awk 如何知道用空格来分隔字段的呢? 因为有一个内部变量 FS 来确定字段分隔符。初始 时，FS 赋为空格
  - (4)awk 打印字段时，将以设置的方法使用 print 函数打印，打印时逗号映射为OFS默认为空格
 
##### 字段解释
  
        $0: awk 变量$0 保存当前记录的内容 # awk -F: '{print $0}' /etc/passwd
        NR: The total number of input records seen so far. # awk -F: '{print NR, $0}' /etc/passwd /etc/hosts  （序号值）
        FNR: The input record number in the current input file # awk -F: '{print FNR, $0}' /etc/passwd /etc/hosts
        NF: 保存记录的字段数，$1,$2...$100 # awk -F: '{print $0,NF}' /etc/passwd
        $NF:因为NF是数字，这个就是最大数的变量值即最后一个字段值
        FS: 输入字段分隔符，默认空格 # awk -F: '/alice/{print $1, $3}' /etc/passwd
        # awk -F'[ :\t]' '{print $1,$2,$3}' /etc/passwd
        # awk 'BEGIN{FS=":"} {print $1,$3}' /etc/passwd
        OFS: 输出字段分隔符 # awk -F: '/alice/{print $1,$2,$3,$4}' /etc/passwd
        # awk 'BEGIN{FS=":"; OFS="+++"} /^root/{print $1,$2,$3,$4}' /etc/passwd
        RS The input record separator, by default a newline. # awk -F: 'BEGIN{RS=" "} {print $0}' a.txt
        ORS The output record separator, by default a newline. # awk -F: 'BEGIN{ORS=""} {print $0}' passwd



- awk是**逐行**处理的，处理完一行再处理下一行，默认以“换行符”为标记，识别每一行。   

- **awk可用于拼接**。被拼接字符串用双引号括起来 如：awk'{print "column1:"$1,"column2:"$2}',注意$1这种内置变了不能使用双引号，否则被当做普通文本输出。
- **awk变量**: 分为**内置变量**和自定义变量

- **过滤操作** ：结合运算符在action中判断如awk '$1>20' log.txt 输出第一列值大于2的记录




  >细心如你一定注意到了一个细节，就是在打印 $0 , $1, $2 这些内置变量的时候，都有使用到"$"符号，但是在调用 NR , NF 这些内置变量的时候，就没有使用"$"，如果你有点不习惯，那么可能是因为你已经习惯了使用bash的语法去使用变量，在bash中，我们在引用变量时，都会使用$符进行引用，但是在awk中，只有在引用$0、$1等内置变量的值的时候才会用到"$",引用其他变量时，不管是内置变量，还是自定义变量，都不使用"$",而是直接使用变量名。
  
  
###### 匹配记录
 
      # awk -F: '$1 ~ /^alice/' /etc/passwd
      # awk -F: '$NF !~ /bash$/' /etc/passwd

###### 比较表达式

条件表达式
    # awk -F: '$3>300 {print $0}' /etc/passwd
    # awk -F: '{ if($3>300) print $0 }' /etc/passwd
    # awk -F: '{ if($3>300) {print $0} }' /etc/passwd
    # awk -F: '{ if($3>300) {print $3} else{print $1} }' /etc/passwd

算数运算

    # awk -F: '$3 * 10 > 500' /etc/passwd
    # awk -F: '{ if($3*10>500){print $0} }' /etc/passwd

逻辑运算

    # awk -F: '$1~/root/ && $3<=15' /etc/passwd # awk -F: '$1~/root/ || $3<=15' /etc/passwd
    # awk -F: '!($1~/root/ || $3<=15)' /etc/passwd

###### 流程控制
    awk -F: '{if($3==0){i++} else if($3>999){k++} else{j++}} END{print i; print k; print j}' /etc/passwd
    
    [root@tianyun ~]# awk -F: '{i=1; while(i<=10) {print $0; i++}}' /etc/passwd //将每行打印 10 次
    
    [root@tianyun ~]# awk -F: '{ for(i=1;i<=10;i++) {print $0} }' /etc/passwd //将每行打印 10 次


  