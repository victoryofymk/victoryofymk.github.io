---
title: VUE(三)：安装vue-cli 3.x
date: 2018-10-25 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## 安装
### 安装vue-cli
```
npm install --g @vue/cli
```
### 创建模板项目
```
vue create 【项目名称】
```
### 项目结构 
通过cli 3.x 新建的项目的结构较之前简单，只有 public 和 src 两个文件夹
### 安装依赖
```
npm install
```
### 配置启动脚本(packge.json > script)
```
"scripts": {
    "dev": "vue-cli-service serve --open",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    "test:unit": "vue-cli-service test:unit",
    "test:e2e": "vue-cli-service test:e2e"
  },
# 可参考文档 https://cli.vuejs.org/guide/cli-service.html
```
### 执行脚本
```
# 选择执行dev环境脚本
npm run serve
```
### 调整webpack配置 
在根目录下新建文件vue.config.js 
文档参考 https://cli.vuejs.org/guide/webpack.html 
在根目录下新建.env .env.development .env.production文件，并修改启动脚本，实现加载不同环境下配置 

vue.config.js参考配置
```
const path = require('path')
const resolve = dir => {
    return path.join(__dirname, dir)
}
const BASE_URL = process.env.BASE_URL
// 参见:https://cli.vuejs.org/guide/webpack.html
module.exports = {
    baseUrl: BASE_URL, // 根域上下文目录
    outputDir: 'dist', // 构建输出目录
    // assetsDir: 'assets', // 静态资源目录 (js, css, img, fonts)
    // pages: vueConf.pages,
    lintOnSave: true, // 是否开启eslint保存检测，有效值：ture | false | 'error'
    runtimeCompiler: true, // 运行时版本是否需要编译
    transpileDependencies: [], // 默认babel-loader忽略mode_modules，这里可增加例外的依赖包名
    productionSourceMap: false, // 是否在构建生产包时生成 sourceMap 文件，false将提高构建速度
    //加载器
    chainWebpack: config => {
        config.resolve.alias
            .set('@', resolve('src'))
            .set('_c', resolve('src/components'))
    },
    devServer: {
        open: true,
        host: 'localhost',
        port: 8080,
        https: false,
        hotOnly: false,
        //代理
        proxy: null,
        before: app => {}
    }
}
```
### 添加其他组件
```
# 如安装包 @vue/cli-plugin-eslint
vue add @vue/eslint
# 如安装包 @foo/vue-cli-plugin-bar
vue add @foo/bar
#安装vuex 和 vue-router是例外
vue add router
vue add vuex
```
### 常用组件
```
可发布生产(dependencies):
    vue // 包含vue-loader（必要组件）
    vue-router // vue 路由 （重要组件）
    vuex // 全局响应式状态仓库 （重要组件）
    axios // vue http请求 （重要组件）
    iview // iView UI框架
    iview-area // 基于Iview的城市级联组件
    clipboard // 轻量级的实现复制文本到剪贴板功能的JavaScript插件
    codemirror // 文本编辑器插件
    countup // 数字数据的动画
    cropperjs //图像裁剪器。
    echarts // 图表
    html2canvas  //浏览器截图插件 
    js-cookie // 轻量级的JavaScript API，用于处理cookie
    simplemde // Markdown编辑器
    sortablejs // 重新排序的拖放列表的JavaScript库。
    vue-i18n // vue国际化
    wangeditor // 轻量级web富文本编辑器
仅在开发环境(devDependencies):
    vue-template-compiler // vue-template组件（必要组件）,版本必须与vue保持一致
    @vue/cli-plugin-babel // ES标准转换组件(必要组建)
    @vue/cli-service // cli-service服务，如运行 构建 测试(必要组件)
    less less-loader // less-loader(必要组件),将less编译为css,将css文件当做模块来处理 style标签里加上lang=”less”里面就可以写less的代码了style标签里加上scoped表示只在此作用域有效
    @vue/cli-plugin-eslint @vue/eslint-config-standard // 语法检查
    eslint-plugin-cypress lint-staged // 用于测试环境的语法检查插件
    @vue/cli-plugin-unit-mocha // 使用mocha-webpack + chai运行单元测试。
    @vue/test-utils // Vue.js的官方测试库。它提供了单元测试Vue组件的方法。
    chai // Chai是节点和浏览器的BDD / TDD断言库，可以与任何javascript测试框架配对。
    mockjs //模拟数据生成器，帮助前端开发和原型与后端进程分离，并在编写自动化测试时特别减少一些单调性。
```

执行脚本
```
# 不是'@vue/cli-plugin-'开头的组件用以下安装方式
npm install -save -dev @vue/eslint-config-standard vuex iview echarts mockjs
# 是'@vue/cli-plugin-【name】'开头的组件可以以下方式安装
vue add name
```

## 安装vue-ui
```
Install:

npm install -g @vue/cli
# OR
yarn global add @vue/cli
```
Create a project:
```
vue create my-project
# OR
vue ui
```

安装过程
```
Vue CLI v3.0.5
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, Router, Vuex, CSS Pre-processors
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Less
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N) y
```

启动
```
   yarn serve 
   // OR
   npm run serve
```
启动UI
```
   vue ui
```

## 卸载重装
```
$ sudo npm uninstall -g vue
$ sudo npm uninstall -g vue-cli
$ sudo npm uninstall -g @vue/cli
$ sudo npm cache clean --force
$ sudo npm install -g vue
$ sudo npm install -g @vue/cli
```

