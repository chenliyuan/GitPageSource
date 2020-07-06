title: Vue父子组件传值
author: 躲不掉的风
date: 2020-05-08 15:09:01
tags:
---
##### 父组件传参给子组件
父组件通过`属性`的形式向子组件传值
步骤：
1. 在父组件调用子组件时通过自定义属性名传递数值
2. 在子组件中定位使用props接收该属性
3. 在子组件中使用该属性

![upload successful](/images/pasted-112.png)

##### 子组件传参给父组件

子组件通过`事件触发`的形式向父组件传值
步骤：
1. 子组件中调用触发自身事件
2. 子组件自身定义的事件通过$emit监听(父组件调用子组件时的)变量    
	（其中$emit两个参数，第一个事件名，第二个传递的参数）
3. 父组件调用子组件的变量通过父组件自身定义的方法接收使用值
	

子组件通过某事件传递给指定getsearchvalue：
```
  <el-form-item style="padding-top:0px">
     <el-button type="info" @click="childbyvalue">Search</el-button>
  </el-form-item>
      ...
  childbyvalue() {
     this.$emit("getsearchvalue", {
        date: this.selecteddate,
        selectedproject: tmp,
        allprojects: this.allprojects
      }
  }
```
父组件监听getsearchvalue

执行getlivebugs方法参数为子组件传递过来的对象：
```
    <div>
      <Searchbydate v-on:getsearchvalue="getlivebugs"></Searchbydate>
    </div>
    ...
   getlivebugs(searchResult) {
      this.selecteddate = searchResult.date;
      ...
      }
```

![upload successful](/images/pasted-113.png)

