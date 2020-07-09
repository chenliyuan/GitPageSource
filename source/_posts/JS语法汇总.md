title: JS语法使用小记
author: 躲不掉的风
date: 2020-06-18 18:09:11
tags:
---
1. JS合并两个数组
    	a.push.apply(a,[4,5,6]);    
       
2. JS 排序功能  sort()
	
    - 【原理】当数组长度<=10的时候，采用插入排序；>10的时候，采用快排。(当数据量很小的时候，插入排序效率优于快排)
    
    对于长度大于1000的数组，采用的是快排与插入排序混合的方式进行排序的.
     
	- array.sort(fun)；参数fun可选，规定排序顺序。默认字母排序
    
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
      
3. filter()函数

	和map()类似，Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

        <script type="text/javascript">
                var arr = [0,1,2,3,4,5,1,4,0];
                var arr_filter = arr.filter(function(x){
                    return x%2 == 0;/* 筛选偶数 */ 
                })
                console.log(arr_filter)
            </script>
	参考：https://www.cnblogs.com/jianxian/p/10582683.html
    
    推荐：https://www.cnblogs.com/saifei/p/9043821.html

4. ES箭头函数

  基本形式：
  ```
  let sum = (num1,num2) => num1 + num2;

  ```
  
8. 引用类型和值类型
    ```
    let  tmp=this.a;   //this.a=[{pj:'listing'}}]
    console.log("初始化的值：",tmp,this.a);
    tmp['aa']=111
    ```
    此段代码在console.log中返回：[{pj:'listing',aa:111}}]  [{pj:'listing',aa:111}}]

	问题：  
    1：为何tmp在打印的时候值已经变化？难道先执行下面再执行上面？   
    2：为何tmp变化会引发a值的变化
    
    解答：
    
	1. ![upload successful](/images/pasted-120.png)
    如下代码片段
    ```
      var json = {a:1,b:2} 
      console.log(json.a)//打印出来是3，不是1
      json.a = 3;
		```
    2. 赋值操作其实引用的都是同一块地址内容，所以一个改动会影响另一个取值的变化
    
    这里使用深拷贝解决
    深浅拷贝：https://www.cnblogs.com/panrui1994/p/9378696.html
    
    **深拷贝方法1**: Object.assign(target, …sources)  对象
    
    【注】assign方式用于对象方可实现深拷贝，[]不可以，因为内部若有对象，其对象依旧是引用类型可以变化)
    
    **深拷贝方法2**: JSON.parse(JSON.stringify(obj))
    
    最终使用JSON.parse实现(JSON.stringify(obj)就是把对象转成字符串，然后JSON.parse就是把字符串再变为对象，那就完全是一个新的对象了，所有的东西都是新的
6. console.log的常用语法
   - console.table，格式化表格
   - 统计代码执行时间
   ```
    console.time('统计时间')
    for(var i = 0; i < 999 ;i++){}
    console.timeEnd('统计时间'
    ```

 