title: Go语言
author: 躲不掉的风
date: 2020-02-03 19:56:22
tags:
---
#### IDE

官网下载atom，并安装go-plus插件


#### 运行程序

		go run hello.go
		或者
        $ go build hello.go 
        $ ls
        hello    hello.go
        $ ./hello
        
#### 特点:与其他语言差别
- 应用程序入口
	1. 必须是 main 包：package main
	2. 必须是 main ⽅法：func main()
	3. ⽂件名不⼀定是 main.go
- Go 中 main 函数不⽀持任何返回值  
  通过 os.Exit 来返回状态
  
-  main 函数不⽀持传⼊参数 func main(arg []string) 是错误的      
  在程序中直接通过 os.Args 获取命令⾏参数 
  
- 编写测试程序

  1. 源码⽂件以 _test 结尾：xxx_test.go
  2. 测试⽅法名以 Test 开头：func TestXXX(t *testing.T){}
  
  运行测试程序：
  
  1）Atom->preferences->packages
  搜索找到go-plus
  在settings中，Test配置中选中 Run with verbose flag setting  
  2）若不好用，可以用修改快捷键绑定keymap.cson文件,先执行保存再使用快捷键运行
  
- 不支持隐式类型转换，如Int和Int64。别名和原有类型也不能进⾏隐式类型转换
-  不支持指针运算
-  运算符之后后置++和--
-  &^ 运算符：只要后面是1，结果变成0，否则为左边值即不变

      1 &^ 0 -- 1
      1 &^ 1 -- 0
      0 &^ 1 -- 0
      0 &^ 0 -- 0
- string 是值类型，其默认的初始化值为空字符串，⽽不是 nil

-  Go 语⾔仅⽀持循环关键字 for
-  条件语句
  1. condition 表达式结果必须为布尔值
  2. ⽀持变量赋值：
  ```
      if  var declaration;  condition {
          // code to be executed if condition is true
      }
```
-  switch条件

  1. 条件表达式不限制为常量或者整数；
  2. 单个 case 中，可以出现多个结果选项, 使⽤逗号分隔；
  3. Go 语⾔不需要⽤break来明确退出⼀个 case；
  4. 可以不设定 switch 之后的条件表达式，在此种情况下，整个 switch 结构与多个 if…else… 的逻辑作⽤等同

#####  数组

  - 数组的声明
 ``` 	
      var a [3] int //声明并初始化为默认零值
      a[0] = 1
      b := [3]int{1, 2, 3} //声明同时初始化
 ```
 
  - 数组元素遍历


        func TestTravelArray(t *testing.T) {
         a := [...]int{1, 2, 3, 4, 5} //不指定元素个数
         for idx/*索引*/, elem/*元素*/ := range a {
         fmt.Println(idx, elem)
         }


- 数组截取

        a := [...]int{1, 2, 3, 4, 5}
        a[1:2] //2
        a[1:3] //2,3
        a[1:len(a)] //2,3,4,5
        a[1:] //2,3,4,5
        a[:3] //1,2,3
  	
    负数索引？不支持
  
##### Go 语言切片(Slice)
Go 语言切片是对数组的抽象。

Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。 

 - 初始化同数组，缺少长度 
 - 数组可以比较，切片不可比较
 
  切⽚声明
  
        var s0 []int
        s0 = append(s0, 1)
        s := []int{}
        s1 := []int{1, 2, 3}
        s2 := make([]int, 2, 4)  
       make参数/*[]type, len, cap
       其中len个元素会被初始化为默认零值，未初始化元素不可以访问
       */
	 
 - len() cap()  make()
  
  append() 和 copy() 函数
  
##### Map

- Map 声明

    声明变量，默认 map 是 nil
    	var map_variable map[key_data_type]value_data_type

    使用 make 函数
 	 map_variable := make(map[key_data_type]value_data_type)
    举例：
      m := map[string]int{"one": 1, "two": 2, "three": 3}
      m1 := map[string]int{}
      m1["one"] = 1
      m2 := make(map[string]int, 10 /*Initial Capacity*/)
      
- 在访问的 Key 不存在时，仍会返回零值，不能通过返回 nil 来判断元素是否存在
- Mapi遍历

      m := map[string]int{"one": 1}
      for k, v := range m {
          t.Log(k, v)
      }

- Mapi delete()函数
- Go 的内置集合中没有 Set 实现， 可以 map[type]bool

##### 字符串与字符编码

字符串是只读不可变的byte slice

len()函数得出的是byte的长度而未必是字符长度。

rune()函数:得到unicode编码

常用的函数包：strings strconv
如：split join Atoi  itoA

##### 变量声明
可单独声明，也可以一次声明多个变量：如
  
      var a string = "Runoob"
      var b, c int = 1, 2
      
  - 若没有初始化系统默认为0或者False或者”“
  
  - 也可是不指定类型，系统自动识别
  - 使用因式分解关键字写法
        var (  // 这种因式分解关键字的写法一般用于声明全局变量
          a int
          b bool
      	)
      
  - 也可是省略var,使用:=来声明新变量
	
    	vname1, vname2, vname3 := v1, v2, v3
        
##### 常量：iota
 
 iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每<font color=red>新增一行</font>常量声明将使 iota 计数一次

	package main
    import "fmt"
    func main() {
        const (
                a = iota   //0
                b          //1
                c          //2
                d = "ha"   //独立值，iota += 1
                e          //"ha"   iota += 1
                f = 100    //iota +=1
                g          //100  iota +=1
                h = iota   //7,恢复计数
                i          //8
        )
        fmt.Println(a,b,c,d,e,f,g,h,i)
    }


###### 记录

	t.Logf("%T", aP)