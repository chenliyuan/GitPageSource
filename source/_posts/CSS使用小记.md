title: CSS使用小记
author: 躲不掉的风
date: 2020-06-22 11:25:20
tags:
---
1. box-shadow 参数(除了color其他单位都是px)

        h-shadow	必需的。水平阴影的位置。允许负值
        v-shadow	必需的。垂直阴影的位置。允许负值
        blur	可选。模糊距离
        spread	可选。阴影的大小
        color	可选。阴影的颜色
        inset
        
  前两项横竖方向决定了投影方向
  
  blur 值越大阴影越模糊
  
  spread值越大范围越大

  ```
  box-shadow:10px 10px 5px 20px #ee4d2d
  ```
  ![upload successful](/images/pasted-116.png)  

  加inset效果：

  ![upload successful](/images/pasted-117.png)
  
2. padding 设置方向，逆时针（即上 右 下 左）

  ![upload successful](/images/pasted-115.png)
  
  若只有两项，则是上下、左右；一项是所有
  
  margin同理
3. align和text-align的区别
  align直接写在是div的属性 
  ```
        <div align="center"> 
        This is some text! 
        </div> 
  ```
  text-align则是Css的属性
  ```
  <div style="text-align:center">
  ```