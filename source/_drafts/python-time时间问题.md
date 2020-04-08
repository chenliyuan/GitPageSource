title: python time时间问题
author: 躲不掉的风
date: 2020-04-02 16:33:05
tags:
---
  https://www.cnblogs.com/pyxiaomangshe/p/7918850.html
  	import time 
    t = "2017-11-24 17:30:00"
    #将其转换为时间数组 
    timeStruct = time.strptime(t, "%Y-%m-%d %H:%M:%S") 
    #转换为时间戳: 
    timeStamp = int(time.mktime(timeStruct)) 
    print(timeStamp)