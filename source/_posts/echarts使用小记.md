title: echarts使用小记
author: 躲不掉的风
date: 2020-06-22 14:12:45
tags:
---
1. echarts及option配置应用

        import echarts from 'echarts';
        Vue.prototype.$echarts = echarts;
        
        import { bar_option, pie_option, line_bugrate_option } from '@/components/chart_options/echart_options.js';
        Vue.prototype.$echartoptions = { bar_option, line_bugrate_option, pie_option };
        使用
        myChart.setOption(this.$echartoptions.line_bugrate_option);

2. echarts的option内赋值，紧接着在外面值就为空了？
	
    this和that的问题，option内的this指的是option本身，非本组件
    
2. 如何限制柱形图的条状宽度(竖着)？

  barMaxWidth barMinWidth
  
3. echarts饼图提示线长度设置

          labelLine:{
          normal:{
              length:1,//第一段，紧跟饼图
              length2:6//第二段，距离最远
          }
      },
  echarts icon图标格式只支持两种，dataURI和path://

  对于多个path构成的矢量图，可采用dataURL方式，在线很方便转换。
  
4. Echarts图表重复问题

	除了数据源需要重置外，echarts需要重置myChart.clear();
    
5. 区分横轴不同文案展示，在xAxis下设置axisLabel：
   ```
   axisLabel= {
            formatter: function(value, index) {
              if (value.search("QA") != -1) return "{a|" + value + "}";
              else if (value.search("BE") != -1) {
                return "{b|" + value + "}";
              } else {
                return "{c|" + value + "}";
              }
            },
            rich: {
              a: {
                color: "red",
                height: 40,
                fontWeight: "bold",
              },
              b: {
                color: "blue",
                height: 40,
                fontWeight: "bold",

              },
              c: {
                color: "green",
                 height: 40,

                fontWeight: "bold"
              }
            }
          
        };
   ```
10. 自定义提示窗口
 	```
    toolbox: {
          feature: {
            myTool2: {
              show: true, //是否显示
              title: "近半年趋势分析", //鼠标移动上去显示的文字     "image://data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAFHklEQVRoQ+1Ze0xbVRj/fS2",
              onclick: function() {
                that.piechartname = chartname;
                that.titlename = "近6个月" + title + "趋势图";
                that.dialogVisible = true;
              }
            }
          }
        }
      };
    ```
11. 为图标添加自定义图标，点击后弹出说明文案，且可换行
	```
        myTool1: {
              show: showmytools1, //是否显示
              title: "信息说明", //鼠标移动上去显示的文字
              ...
              onclick: function() {
                that.$confirm(h("div", null, msgli), headtitle, {
                  showConfirmButton: false,
                  showCancelButton: false,
                  type: "info"
                });
              }
    ```
    其中文案的处理
    ```
      let msgli = [];//最终的显示文本
      let h = this.$createElement;
      let causeText = [];
      let headtitle = title + "说明";
      switch (chartname) {
        case "bug_role":
          causeText = [
            "根据角色分析Bug分布情况，主要包括QA、FE、BE、PM",
            "QA: 取QA字段"
          ];
          for (const i in causeText) {
            msgli.push(h("p", null, causeText[i]));
          }
          showmytools1 = true;//控制是否展示
          break;
    ```
    另外一种方式:使用el-dialog
    
    先定义一个el-dialog
    ```
        <el-dialog :title="direction_title" :visible.sync="noteVisible" width="25%" 
    top="15%"  :show-close="false">
      <div v-html="direction_content"></div>
    </el-dialog>
    ```
    在onclick自定义事件中:
    ```
    that.direction_title = title + "说明";//在option中使用that
    switch (chartname) {
   		case "bug_role":
            that.direction_content ="<span>说明文字1</span>" //需要在click事件中赋值，否则出错
            ....
         }
    that.noteVisible = true;//控制显示el-dialog
    ```
3. 自定义的说明图标点击后文字this.direction_content展示的是最后一个饼图的数据？

	【解决】
     - 在Onlick的时候再赋值 (尤其针对这种全局共享的语法糖)    
     - 在内部引用，使用that定义