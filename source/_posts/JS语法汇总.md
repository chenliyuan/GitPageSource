title: JS语法使用小记
author: 躲不掉的风
date: 2020-06-18 18:09:11
tags:
---
1. JS合并两个数组
    	a.push.apply(a,[4,5,6]);    
       
2. JS 排序功能

	 array.sort(fun)；参数fun可选，规定排序顺序。默认字母排序
    
     示例：
       var arr3 = [30,10,111,35,1899,50,45];
       arr3.sort(function(a,b){
        return a - b;
       })
       console.log(arr3);//输出 [10, 30, 35, 45, 50, 111, 1899]
       
      多属性判断排序(在project&role相同的情况下才排序)
      ``` 
          export function compare(property, index, type) {
          return function(a, b) {
              if (a.project == b.project && a.role == b.role) {
                  var value1 = a[property][index];
                  var value2 = b[property][index];
                  if (type == "ascending") {
                      return value1 - value2;
                  } else {
                      return value2 - value1;
                  }
              }
          }
      }
      ```
      
     具体可参考：https://www.cnblogs.com/lyc10/p/11348419.html
      
6. filter()函数

	和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

        <script type="text/javascript">
                var arr = [0,1,2,3,4,5,1,4,0];
                var arr_filter = arr.filter(function(x){
                    return x%2 == 0;/* 筛选偶数 */ 
                })
                console.log(arr_filter)
            </script>
	参考：https://www.cnblogs.com/jianxian/p/10582683.html