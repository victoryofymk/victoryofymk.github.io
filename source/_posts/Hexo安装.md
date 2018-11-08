---
title: Hexo(一)：安装 hexo
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 常用依赖
• Hexo - 快速、简洁且高效的博客框架
• Github Pages - 完美的静态博客服务器（除大陆网速慢外）
• NexT - 一款高质量且简洁优雅的Hexo主题
• Travis CI - 一个持续集成测试工具
## 安装依赖

搭建环境：macos

1.安装node
```
Brew install node
```
2.安装git
```
brew install git
```
3.安装 hexo
```
npm install -g hexo 
```
## 初始化
终端cd到一个你选定的目录，执行hexo init命令：
```
$ hexo init blog
```
blog是你建立的文件夹名称。cd到blog文件夹下，执行如下命令，安装npm：
```
$ npm install
```
执行如下命令，开启hexo服务器：
```
$ hexo s
```
此时，浏览器中打开网址http://localhost:4000

## 基本操作

终端cd到blog文件夹下，执行如下命令新建文章：
```
#名为postName.md的文件会建在目录/blog/source/_posts下
hexo new "postName" 
```
在blog文件夹目录下执行生成静态页面命令：
```
$ hexo generate     或者：hexo g  
```

此时若出现如下报错：
```
ERROR Local hexo not found in ~/blog
ERROR Try runing: 'npm install hexo --save'
```

则执行命令：
```
$ npm install hexo --save
```
若无报错，自行忽略此步骤。


## 重新部署 
hexo clean：清空public
hexo g：静态文件重新生成
hexo d：部署

hexo generate hexo deploy 简写 hexo g -d



