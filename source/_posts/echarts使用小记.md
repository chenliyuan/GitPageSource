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
 