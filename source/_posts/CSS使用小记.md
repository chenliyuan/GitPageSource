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
  
4. 布局
footer如何保持始终在最底部？https://blog.csdn.net/m0_38099607/article/details/71598423

	父级：
		#container{
        /*保证footer是相对于container位置绝对定位*/
        position:relative;  
        width:100%;
        min-height:100%; 
        /*设置padding-bottom值大于等于footer的height值，以保证main的内容能够全部显示出来而不被footer遮盖；*/  
        padding-bottom: 100px;  
        box-sizing: border-box;
    }
    
	footer要绝对位置		

        footer{
        width: 100%;
        height:100px;   /* footer的高度一定要是固定值*/ 
        position:absolute;
        bottom:0px;
        left:0px;
        background: #333;
    }
    
 使得全局撑足屏幕？
 
 html文件中添加，使用的container100%才生效
 
       <style type="text/css">
          html,
          body {
              height: 100%;
          }

          body {
              margin: 0;
          }
      </style>
      
5. div居中

 内部div需要添加
 		
        margin:0 auto