title: elementui使用小记
author: 躲不掉的风
date: 2020-06-24 11:26:36
tags:
---
1. 需求：表格上拉加载刷新

  尝试单独使用InfiniteScroll 无限滚动 解决，但是一直存在两个问题
    1. 每次重复执行3次
    2. 第二次上拉时不加载
    
  猜测由于表格特定格式单纯使用InfiniteScroll 受限
  
  【解决】使用```el-table-infinite-scroll```指令结合elmentui中```infinite-scroll-disabled="disabled"```

   参考：https://zhuanlan.zhihu.com/p/114965548