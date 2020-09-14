title: JS语法使用小记
author: 躲不掉的风
date: 2020-06-18 18:09:11
tags:
---
js数组操作方法以及es6新增方法:https://www.cnblogs.com/Ky-Thompson23/p/12408795.html

1. JS合并两个数组 

  apply会改变原数组的值
    	a.push.apply(a,[4,5,6]);  
  concat不会改变原数组的值
       arr1 = [1,2];
       arr2 = ["aa","as"];
       console.log(arr1.concat(arr2));//[1, 2, "aa", "as"]
  
2. 删除数组某项	

	splice(index,len,[item]) 注释：该方法会改变原始数组。
    index:数组开始下标 
    
    len: 替换/删除的长度
    
    item:替换的值，删除操作的话 item为空
	```
    var arr = ['a','b','c','d']; 
    arr.splice(1,1); 
    console.log(arr); 
    //['a','c','d'];
    ```
    删除某值元素
    ```
    a=[1,2,3]
    a.splice(a.indexOf(2),1)// [1,3]
    ```
    参考 https://www.jb51.net/article/134312.htm
    
   添加 ---- len设置为0，item为添加的值 
   
   ```
   var arr = ['a','b','c','d']; 
  arr.splice(1,0,'ttt'); 
  console.log(arr); 
  //['a','ttt','b','c','d'] 表示在下标为1处添加一项'ttt' 
   ```
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
        var summary = origin_outline
          .replace(/<\/p>/g, "")
          .split("<p>")
          .filter(function(s) {
            return s && s.trim();
          })
          .map(function(m) {
            return m.trim();
          });
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
7.  数组对象筛选
 
       ```this.materialss = this.materials.filter(item => item.categoryId === this.curTab.categoryId)```      
8. 对象列表排重
      ```
      function unique(arr,field){
          var map = {};
          var res = [];
          for (var i = 0; i < arr.length; i++) {
              if (!map[arr[i][field]]) {
                  map[arr[i][field]]=1;//巧妙利用对象键值唯一性判重
                  res.push(arr[i]);
              }
          }
           return res;
       ```
  原文链接：https://blog.csdn.net/qq_36949713/java/article/details/81942370
  
10. JS Base64转码

```
let Base64 = {
    encode(str) {
        // first we use encodeURIComponent to get percent-encoded UTF-8,
        // then we convert the percent encodings into raw bytes which
        // can be fed into btoa.
        return btoa(encodeURIComponent(str).replace(/%([0-9A-F]{2})/g,
            function toSolidBytes(match, p1) {
                return String.fromCharCode('0x' + p1);
            }));
    },
    decode(str) {
        // Going backwards: from bytestream, to percent-encoding, to original string.
        return decodeURIComponent(atob(str).split('').map(function(c) {
            return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
        }).join(''));
    }
};
```
对于json数据，先转换成字符串
Base64.encode(JSON.stringify(this.result))