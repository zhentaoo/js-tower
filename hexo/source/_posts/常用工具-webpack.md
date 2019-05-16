---
title: 前端工具-webpack
date: 2017-02-23 20:11:49
categories: 前端工程化
---
不多说吧，先贴一份完整的webpack配置文件，如果有问题可以加群讨论微博私信

webpack.config.js
```js
var webpack = require('webpack');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    index: './src/index.js',
    vendor: ['moment', 'lodash']
  },
  output: {
    path: path.resolve(__dirname, './assets'),
    filename: '[name].[hash:8].js'
  },
  module: {
    loaders: [
      {test: /\.js$/,loader: 'babel-loader',exclude: /node_modules/},
      {test: /\.css$/, loader: "ExtractTextPlugin.extract('css') : 'style!css';"}
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    }),
    new ExtractTextPlugin("styles.css")，
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor'
    }),
    new webpack.optimize.UglifyJsPlugin({
      beautify: false,
      mangle: {
       screw_ie8: true,
       keep_fnames: true
      },
      compress: {
       screw_ie8: true
      },
      comments: false
    })
  ]
};
```

### webpack性能优化
实际上webpack提供了大量提升打包速度，文件大小的插件。
https://webpack.js.org/plugins/
#### 多入口打包:
如果把所有文件打包成一个bundle.js，会导致这个文件过大，
而且无法利用浏览器并发特性，不能同时获取多个JS文件，显然不合适。
所以可以使用多文件打包方式，也就是在entry配置多个入口

#### CommonsChunkPlugin:
当使用多入口方式打包时候，会把公共模块同时打包到两个文件中！！！
显示不合理，所以使用CommonsChunkPlugin，将公共模块只打包到vendor.js中

#### UglifyJsPlugin:
会将打包的文件进行最小化的压缩，可以尽可能的减小文件体积，开发过程不建议开启。
我上面的配置文件是写在一起了，😄
不过各位可以使用DefinePlugin定义变量然后区分开发、生产环境。

#### ExtractTextPlugin:
类似多入口，CSS文件如果打包在JS中，JS文件会很大
使用ExtractTextPlugin将CSS文件单独打包出来
