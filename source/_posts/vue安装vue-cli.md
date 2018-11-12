---
title: VUE(二)：安装vue-cli
date: 2018-10-24 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## 安装vue-cli
假设你的机子上已经有了最新的node和npm了，那我们就只需要执行以下命令：
```
$ npm install -g vue-cli
```
构建完了之后，随便进入一个我们事先准备好的目录，比如demo目录，然后在目录中做初始化操作：
```
# vue init 是vue初始化项目
# webpack 是打包工具，其中也可以使用Browserify，感兴趣可以自行研究
# project 是自定义的项目名称
$ vue init webpack myProject
```

webpack参数是指myProject这个项目将会在开发和完成阶段帮你自动打包代码，比如将js文件统一合成一个文件，将CSS文件统一合并压缩等。

安装中的提示信息,可以修改和选择
```

? Project name vueproject   ---------------------项目名称
? Project description A Vue.js project---------------------项目描述
? Author wondershoter <starlord.yan@gmail.com>---------------------作者
? Vue build standalone
? Install vue-router? Yes---------------------是否安装Vue路由，推荐安装，是页面跳转用的
? Use ESLint to lint your code? Yes---------------------是否启用eslint检测规则
? Set up unit tests no---------------------是否安装单元测试,选择否，不然安装依赖会卡住
? Setup e2e tests with Nightwatch? Yes---------------------是否安装e2e测试
? Should we run `npm install` for you after the project has been created? (recommended) npm
```
按照提示信息启动
```
# 进入工程目录
cd myProject
# 安装依赖，如果要具体安装其他模块可以单独使用，执行之后，目录里多了一个node_modules文件夹，这里放的就是所有依赖的模块
npm install #如果安装过程已经执行，略过；
# 运行项目
npm run dev 
# 打包工作，用于正式环境
npm run build
```
npm run dev 是开始执行我们的项目了，一旦执行这个命令之后，等一小会，浏览器应该会自动帮你打开一个tab为http://localhost:8080/#/的链接，这个链接就是我们本地开发的项目主页

修改端口
```
```


## 文件目录分析
package.json保存一些依赖信息，config保存一些项目初始化配置，build里面保存一些webpack的初始化配置，index.html是我们的首页，除了这些，最关键的代码都在src目录中，index在很多服务器语言中都是预设为首页，像index.htm，index.php等
```
build    -------------------项目构建相关代码（webpack配置）
    build.js  -------------生产环境构建代码
    check-versions.js  ----------检查node、npm等版本
    utils.js  ------------------------构建工具相关
    vue-loader.conf.js  ---------css加载器的配置
    webpack.base.conf.js  ---webpack的基础配置信息
    webpack.dev.conf.js  -----webpack开发环境配置信息，构建开发本地服务器
    webpack.prod.conf.js  ---wenpack生产环境配置信息
config    -------------------配置目录，包括端口号，打包输出等的vue基本配置文件
    dev.env.js  -----------开发环境变量
    prod.env.js -----------生产环境变量
    index.js  -------------项目的配置变量，端口号等 
node_modules    -----------加载的项目依赖模块
static    -------------------静态资源目录
index.html    ---------------首页的入口文件，可以添加meta等参数
README.md    ---------------项目的说明文档，makedown格式
src    -----------------------源码目录，主要的开发
    assets  ---------------静态资源，css,image等可以存放
    components  -----------公共组件
    router  ---------------路由文件夹，配置页面跳转
    views  ----------------页面编写的地方，（可以自行定义命名）
main.js  ------------------入口文件，全局的配置和加载
.babelrc  -----------------ES6语法编译配置，用来将es6代码转换成浏览器可识别的es5代码
.gitignore  ---------------git上传需要忽略的文件的格式
package.json  -------------项目的基本信息，包括开发所需要的模块、项目名称、版本号等
.postcssrc.js  ------------转换css的工具
.editorconfig  ------------定义代码格式
```

入口js文件在src目录中的main.js


## 说明
Babel，转译成浏览器可识别的语言，可以让你的项目支持更新的语法，如es6\es7等
PWA，模拟原生app，渐进式网络应用程序




