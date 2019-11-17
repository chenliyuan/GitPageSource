title: awk知识点汇总
date: 2018-08-22 15:23:35
---
参考原文：https://blog.csdn.net/liang5603/article/details/80855426

grep sed awk 被称为linux中的“三剑客”
grep更适合单纯的查找或匹配文本
sed更适合编辑匹配到的文本
awk更适合格式化文本，对文本进行较复杂格式处理
- **awk语法： awk [options]  'pattern{Action}' file  **  

- ##### 【action】
	- awk最常用的动作(action)就是print和printf   
    
  - awk是**逐行**处理的，处理完一行再处理下一行，默认以“换行符”为标记，识别每一行。   
  - awk**默认使用空格作为分隔符**去分割当前行。分割完的第一个字段为$1（不是$0，$0表示所有行）,以此类推。   
  - $NF表示表示分割后的最后一列,NF和$NF不一样，NF表示每行被分割的数量。（NR - Number of Record）

  - **输出多列(多个参数值)**。使用逗号隔开awk  '{print $1,$2,$3}'：结果以空格分开，但是不加逗号会连接在一起输出

  - **awk可用于拼接**。被拼接字符串用双引号括起来 如：awk'{print "column1:"$1,"column2:"$2}',注意$1这种内置变了不能使用双引号，否则被当做普通文本输出。
  - **输出所有行**：awk'{print $0}' 等价于 awk '{print}'
  - **awk变量**: 分为**内置变量**和自定义变量
  - **过滤操作** ：结合运算符在action中判断如awk '$1>20' log.txt 输出第一列值大于2的记录


- ##### 【options】
	- BEGIN，END；在处理文本之前/后执行的操作，eg：

    $ awk '**BEGIN**{print "wolaiceshibegin.."} {print $1,$2}' awktest.txt  
    $ awk '{print $1,$2}**END**{print "test ending"}' awktest.txt
    
  - -F指定分隔符   -v 设置变量的值
   
   - **设置输入分隔符**（列以什么分隔符为准split）：   
   awk -F或者awk -v -FS   
   如    awk -F# '{print $1,$2}'   test.txt  
   相当于 awk -v FS='#' '{print $1,$2}' test.txt

    - **设置输出分隔符**（输出列以什么分隔符join）：   
    awk -v OFS  
    如 awk -v FS="#" -v OFS='---''{print $1,$2}' test.txt (输入和输出一起用，OFS output field seperator)


  如NR代表行号，NF代表每行被分列数：

  >细心如你一定注意到了一个细节，就是在打印 $0 , $1, $2 这些内置变量的时候，都有使用到"$"符号，但是在调用 NR , NF 这些内置变量的时候，就没有使用"$"，如果你有点不习惯，那么可能是因为你已经习惯了使用bash的语法去使用变量，在bash中，我们在引用变量时，都会使用$符进行引用，但是在awk中，只有在引用$0、$1等内置变量的值的时候才会用到"$",引用其他变量时，不管是内置变量，还是自定义变量，都不使用"$",而是直接使用变量名。