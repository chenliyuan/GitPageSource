title: python语法
author: 躲不掉的风
date: 2019-08-09 17:29:00
tags:
---
1.处理小数  参考：https://www.jb51.net/article/102248.htm
- 向下取整 int()
- 四舍五入 round()
```
>>> round(3.25); round(4.85)
3.0
5.0
```
- 向上取整 ceil()
- 分别取整数部分和小数部分
```
>>> import math
>>> math.modf(3.25)
(0.25, 3.0)
>>> math.modf(3.75)
(0.75, 3.0)
>>> math.modf(4.2)
(0.20000000000000018, 4.0)
```