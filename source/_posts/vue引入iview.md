---
title: VUE(四)：引入iview
date: 2018-10-26 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## 环境基础
vue-cli 3.x搭建环境
## 安装

### npm安装
NPM安装 iView
```
npm i -S iview
```
#### 全局引入

一般在 webpack 入口页面 main.js 中如下配置：
```
import iView from 'iview';
import 'iview/dist/styles/iview.css';

Vue.use(iView);
```
會出現 iView is unidefined,正確的配置
src/iview/index.js

import iview from 'iview'
前面的 iview 要用小寫
```
import Vue from "vue";
import iview from "iview";
import "iview/dist/styles/iview.css";

Vue.use(iview);
```
src/main.js加入這一行即可
```
import "./iview";
```

注意：babel.config.js中的plugins配置要删除，否则会引起冲突

#### 按需引入
修改babel.config.js，添加下面配置
```
"plugins": [
    [
      "import",
      {
        "libraryName": "iview",
        "libraryDirectory": "src/components"
      }
    ]
  ]
```
如果您想在 webpack 中按需使用组件，减少文件体积，可以这样写：
```
import { Button, Table } from 'iview';
Vue.component('Button', Button);
Vue.component('Table', Table);
```
**提醒：***按需引用仍然需要导入样式，即在 main.js 
或根组件执行 import 'iview/dist/styles/iview.css';*
        
注意：尽量使用index.js中的命名，避免使用vue内置的名字，否则会引起错误

### vue-ui安装
## 自定义配置：
此时很多朋友会问了，没有配置文件，那我需要自定义一个配置咋办呢？
莫慌，此时我们只需要在项目根目录新建一个 vue.config.js 文件就能使用自定义配置了
```
module.exports = {
  baseUrl: process.env.NODE_ENV === 'production'
    ? '/production-sub-path/'
    : '/',
  devServer: {
    port: 8000
  }
}
```
## 组件使用规范
```
a、动态传值，使用 :prop = ''
b、在非 template/render 模式下（例如使用 CDN 引用时），
   组件名要分隔（驼峰命名改为烤肉串），例如 DatePicker 必须要写成 date-picker
c、以下组件，在非 template/render 模式下，需要加前缀 i-：
    ·Button: i-button
    ·Col: i-col
    ·Table: i-table
    ·Input: i-input
    ·Form: i-form
    ·Menu: i-menu
    ·Select: i-select
    ·Option: i-option
    ·Progress: i-progress
   以下组件，在所有模式下，必须加前缀 i-，除非使用 iview-loader：
     ·Switch: i-switch
     ·Circle: i-circle
```

### 问题
使用箭头函数this为空,箭头函数会改变this指向



Try changing the iView import statement to the following:

import iView from 'iview/dist/iview.min';
Frequently you'll find that distribution packages contain the production version of the library located within a dist directory. Seeing as the stylesheet is located there, I'd assume the js is there as well.

// likely that the iview js in this directory 
import 'iview/dist/styles/iview.css';

