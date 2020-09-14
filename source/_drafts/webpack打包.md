title: webpack打包
author: 躲不掉的风
date: 2020-07-21 10:09:06
tags:
---
##### 插件：CommonsChunkPlugin 抽取的是公共部分
在webpack.pro.conf.js中ew webpack.optimize.CommonsChunkPlugin定义抽离配置：

  - vendor:所有node_modules/下的被require(import)的js文件

  - manifest：webpack运行时代码

  - app.js:基本就是实际编写的那个app.vue
