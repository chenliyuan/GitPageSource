title: shell 通配符和正则表达式
author: 躲不掉的风
date: 2020-02-06 21:08:33
tags:
---
#####  通配符和正则
- shell 元字符(也称为通配符) 由 shell 来解析，如 rm -rf *.pdf，元字符* Shell 将其解析为任意 多个字符

  正则表达式元字符 由各种执行模式匹配操作的程序来解析，比如 vi、grep、sed、awk、python

- shell中定义元字符

      *   任意字符
      ?  单个任意字符
      [] 子shell ，不会影响主shell
      [^abcd]这里^是非的意思，但是外面不支持，不同与正则。正则[]里面同此，外面使用是以什么开头的意思
      {} 集合
      
      
-  基本正则表达式元字符:

        元字符 功能 示例 ======================================================== ^ 行首定位符 ^love
        $ 行尾定位符 love$
        . 匹配单个任意字符 l..e
        * 匹配前导符 0 到多次 ab*love
        .* 任意多个字符
        [] 匹配指定范围内的一个字符 [lL]ove
        [ - ] 匹配指定范围内的一个字符 [a-z0-9]ove
        [^] 匹配不在指定组内的字符 [^a-z0-9]ove
        \ 用来转义元字符 love\.
        \< 词首定位符 \<love
        \> 词尾定位符 love\>
        \(..\) 匹配稍后使用的字符的标签 :% s/172.16.130.1/172.16.130.5/ :% s/\(172.16.130.\)1/\15/
        :% s/\(172.\)\(16.\)\(130.\)1/\1\2\35/
        :3,9 s/\(.*\)/#\1/
        x\{m\} 字符 x 重复出现 m 次 o\{5\}
        x\{m,\} 字符 x 重复出现 m 次以上 o\{5,\} x\{m,n\} 字符x重复出现m到n次o\{5,10\}

- 扩展正则表达式元字符（egrep或使用转义\）

        + 匹配一个或多个前导字符 [a-z]+ove
        ? 匹配零个或一个前导字符 lo?ve
        a|b 匹配 a 或 b love|hate
        () 组字符 loveable|rs love(able|rs) ov+ (ov)+ (..)(..)\1\2 标签匹配字符 (love)able\1er x{m} 字符x重复m次o{5}
        x{m,} 字符 x 重复至少 m 次 o{5,}
        x{m,n} 字符x重复m到n次o{5,10}
- 其他

       \w  所有字母和数字 相当于[a-zA-Z0-9]*
       \W  非字母和数字
       \b  边界
        
##### grep命令

   - 习惯性加上单引号

   - 返回状态exit 0 1 2
   - grep -e  相当于 egrep 

   - grep  --help |grep '\-v'
    其中-v被认为是选项，非模式，所以需要转义



    -i, --ignore-case         忽略大小写
    -v, --invert-match        选中不匹配的行
    -q, --quiet, --silent     不显示所有常规输出
    -r, --recursive           等同于--directories=recurse递归目录
    
    -o, --only-matching       只显示匹配PATTERN 部分的行
    -B, --before-context=NUM  打印文本及其前面NUM 行
    -A, --after-context=NUM   打印文本及其后面NUM 行
    -C, --context=NUM         打印NUM 行输出文本
    
    -n, --line-number         输出的同时打印行号





    
    
    