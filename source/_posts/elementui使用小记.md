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
2. 在加载树形数据懒加载的时候报错 ```if there's nested data,rowKey is required```

	原因：原来使用懒加载，table数据中没有children字段，但是现在新增了这个字段。
  
  因此满足了懒加载的第一种策略：即使我根本没有设置第一项(第一项指的是子元素字段，第二项懒加载的时候使用字段)
  ```:tree-props="{children: 'children', hasChildren: 'hasChildren'}">```
  
  这样直接加载子项目的时候，发现我子项目里缺少id,因此报如上错
 