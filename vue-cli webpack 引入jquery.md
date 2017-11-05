# vue-cli webpack 引入jquery
今天费了一下午的劲，终于在vue-cli 生成的工程中引入了jquery，记录一下。(模板用的webpack)

首先在package.json里的dependencies加入"jquery" : "^2.2.3"，然后install

在webpack.base.conf.js里加入

var webpack = require("webpack")

在module.exports的最后加入

plugins: [
new webpack.optimize.CommonsChunkPlugin('common.js'),
new webpack.ProvidePlugin({
jQuery: "jquery",
$: "jquery"
})
]

然后一定要重新 run dev

在main.js 引入就ok了import $ from 'jquery'

## 或者

引入jq：

输入 npm install jquery --save-dev      用npm下载jq依赖、
  
 找到build文件夹下的webpack.base.conf.js文件打开，修改配置：
   
1、加入webpack对象：

var webpack=require('webpack');

 
 2、在module.exports里面加入：
 
plugins: [  
  new webpack.ProvidePlugin({  
    $:"jquery",  
    jQuery:"jquery",  
    "windows.jQuery":"jquery"  
  })  
]  

3、在入口文件main.js中加入

import $ from 'jquery'

# http://blog.csdn.net/hongchh/article/details/55113751


