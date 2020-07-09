title: vue+Django项目实践问题汇总
author: 躲不掉的风
date: 2020-06-07 20:42:35
tags:
---
1. Django跨域设置  

 pip install django-cors-header   

  setting中设置：

       'corsheaders.middleware.CorsMiddleware' # 新加的插件位置最好放在CommonMiddleware之前      
       INSTALLED_APPS中添加'corsheaders',    
       CORS_ORIGIN_ALLOW_ALL = True   //新增   
       CORS_ALLOW_CREDENTIALS = True  //新增
       
2. 跨域设置后还是不可以？   
/home/getallbus 可以 
getallbus 单层就不可以 ？
3. 接口报403  500 ?

  request.POST.get改成使用字典格式再用get获取   
  返回值是非json格式，在里的对象是键值对，不能缺少键值
 
4. 生命周期中调用方法，因为是方法后面别忘了加()
           this.getallbugs();
5. 修改文件夹和文件名称后找不到？

	把这修改的文件夹删除掉，重新新建了个文件夹
    
6. vue 父子组件的生命周期顺序：加载渲染顺序

    父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mount
    
7. vue中created和mounted的区别

  created：在模板渲染成html前调用，即通常**初始化某些属性值**，然后再渲染成视图。      
  mounted：在模板渲染成html后调用，通常是初始化页面完成后，再对html的**dom节点**进行一些需要的操作。
 
8. axios post传递参数，使用$qs处理

    https://www.cnblogs.com/oukele/p/12670757.html
    axios post传递数组数据？               
    通过indices参数，后台接收使用request.POST.getlist()方法接收
    mains中设置
    ```
    axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'
    ```
    传递参数时：
    ```
    ({    
    'selecteddate':searchResult.date,
    'selectedbus':searchResult.bus
   }, { indices: false });
    ``` 
10. 嵌套路由跳转失败？

   children挂载到了错误的节点上。
11. 渲染chart图标报错？

  在created调用，此时div还没有出来，无法在其基础上加载图片。改成mounted
  
12. 如何修改ui-element样式？

    通过查看元素找到原有样式，再针对性进行修改。       

14. 子组件传已选择项和所有项目，主组件全部接收后判断如果已选选项为空则取所有项，否则取已选选项

 ![upload successful](/images/pasted-114.png)
 
16. 不可以直接绑定由props父组件传递过来的值，否则会报错

  [Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "project"

17. props报错
 Invalid prop: type check failed for prop "value". Expected String, Number, got Array 
 
  【解决】通过将定义的字符串类型转换成列表类型(定义的data属性原被定义成字符串了，所以报错)

18. 多选和单选使用同一个控件控制   
【解决】

   -  写两份下拉框，使用v-show控制multiple属性(控制是否多选)

   -  多选和单选的绑定类型不一样，多选需是list，单选是字符串。然后通过判断来选择（出过错）

20. 引入的css样式没有生效？

  在main.js中引用未全部生效(局部生效，仅限于)，在app.vue中引用集体生效了
  https://blog.csdn.net/HY_358116732/article/details/82219387

   即在app.vue中引入
   ```
   <style>
  @import './../static/css/global.css'; /*引入公共样式*/
   </style>
   ```
23. Vue中使用svg  

  https://zhuanlan.zhihu.com/p/67325098?1=1  

26.  html的link设置浏览器图标，如果不更换正确的图标原缓存很难清除掉，所以说如果新的图标没生效可能就是没设置对

	方法： 设置favicon.ico在static路径(图标有现成的话可以把别人下载下来己用哦)
    
28. ref和refs

    ref 被用来给DOM元素或子组件注册引用信息。引用信息会根据父组件的 $refs 对象进行注册。
    
    如果在普通的DOM元素上使用，引用信息就是元素; 
    
    如果用在子组件上，引用信息就是组件实例,添加两个组件ref
    
    ```
    <el-form :model="form" label-position="right" :rules="rules" ref="adviceForm">

    ```
    打印出两个组件对象：
	![upload successful](/images/pasted-119.png)
    使用($refs引用)：
        this.$refs.adviceForm.resetFields();

30. 我们需要重置该页面的原始数据，每个数据进行重写特别麻烦。利用Object.assign 以及vue的数据可以快速重置。 

		Object.assign(this.$data, this.$options.data())
        
31. 分页实现

        <div class="paginationClass">
          <el-pagination
            @size-change="handleSizeChange" //切换每页最多展示条数
            @current-change="handleCurrentChange"//切换页码
            :page-sizes="[10, 20, 50]"
            :page-size="pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </div>
  方法实现：   
  
         handleSizeChange: function(pagesize) {
            // 每页条数切换
            this.pagesize = pagesize;
            if (pagesize > 10) {
              this.showtablehg = 800;
            }
            this.handleCurrentChange(this.currentPage);
          },

         handleCurrentChange: function(currentPage) {
            //页码切换
            this.currentPage = currentPage;
            this.getallbugs(currentPage);后端实现分页逻辑
            // this.currentChangePage(this.bondsAllList, currentPage);

          },
          前端实现分页逻辑(将当前页的数据取出给table绑定)：
              currentChangePage(list, currentPage) {
              let from = (currentPage - 1) * this.pagesize;
              let to = currentPage * this.pagesize;
              this.tempList = [];
              for (; from < to; from++) {
                if (list[from]) {
                  this.tempList.push(list[from]);
                }
              }
            },
 9. 为表格添加说明

  在el-table-column添加：
       :render-header="renderHeader"
  定义方法：
  ```
   renderHeader(h, { column, $index }) {
        return h(
          "el-tooltip", //  标签的名称
          {
            props: {
              //标签的参数　通过不同的标签 显示不同的文字
              content: (function() {
                //如何拿到 label的文字？？ 通过column.label来拿
                let property = column.property;
                switch (property) {
                  case "live_bugs":
                    return '我是提示文字live_bugs';
                    break;
                  case "提示2":
                    return "提示文字2";
                    break;
                  case "提示3":
                    return "提示文字3";
                    break;
                }
              })(),
              placement: "top"
              // effect: "light",  //  默认为黑色主题
            },
            ///下面的意思是往el-tooltip　标签里面添加内容　column.label（拿到自己定义的显示内容：　拦截状态　）
            domProps: {
              innerHTML:
                column.label +
                '<i class="el-icon-question" style="margin-left:3px;font-size:15px;"/>'
            }
          },
          [h("span")]
        );
      },
  ```           
10. 文件下载


  对流文件可使用 Blob对象 和 使用 js-file-download
  
  发送请求时都要设置 responseType
  
  以下是完整的一个文件下载请求和处理过程：
  ```
  {
      var param = this.$qs.stringify(
        {
          sub_project: this.selectedsubproject,
          src: this.selectedsrc,
          version: this.selectedversion
        },
        { indices: false }
      );
      this.$axios
        .post("version/task/detail/export/", param,{responseType: 'blob'})
        .then(res => {
   
        const content = res.data
        const blob = new Blob([content]) // 构造一个blob对象来处理数据
        const fileName = 'test.xlsx' // 导出文件名
        
        if ('download' in document.createElement('a')) { // 支持a标签download的浏览器
          const link = document.createElement('a') // 创建a标签
          link.download = fileName // a标签添加属性
          link.style.display = 'none'
          link.href = URL.createObjectURL(blob)
          document.body.appendChild(link)
          link.click() // 执行下载
          URL.revokeObjectURL(link.href) // 释放url
          document.body.removeChild(link) // 释放标签
        } else { // 其他浏览器
          navigator.msSaveBlob(blob, fileName)
        }
        })
        .catch(function(err) {
          console.log(err);
        });
    },
  ```
  参考：https://segmentfault.com/a/1190000020540788?utm_source=tag-newest