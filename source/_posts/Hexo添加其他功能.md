---
title: Hexo(八)：添加其他功能
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 为Hexo搜索与统计
搜索与统计都比较简单，[官方文档](http://theme-next.iissnan.com/third-party-services.html#analytics-busuanzi)有详尽的明细,统计推荐不蒜子，简单粗暴。
搜索的话我使用的是本地搜索，即Local Search。他的原理是在你本地生成一个xml文件,搜索的时候对这个文件进行检索。下面说说安装步骤
1.执行下面2个命令
```
npm install hexo-generator-search --save
npm install hexo-generator-searchdb --save
```
2.打开站点配置文件，新增以下内容：
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
3.打开主题配置文件，启用本地搜索功能：
```
# Local search
local_search:
  enable: true
```

## 图片

先来看看hexo官方文档[资源文件夹](https://link.zhihu.com/?target=https%253A//hexo.io/zh-cn/docs/asset-folders.html)

设置好了以后每次你通过 hexo new blognames 在source文件夹下生成一个blognames.md文件以后会自动的生成一个文件夹,该文件夹也是以blognames命名,此时将你想要插入到md中的图片放进去,例如:我放了一个icon.jpg,name你就可以在md中这样引用了
![图片描述](icon.jpg)
注意:按照官方的文档,引用的格式应该是:![图片描述](/blognames/http://icon.jp),其实不然,因为生成的相应的blognames博文和该资源位于同一个文件夹下,因此还是上面的引用方式是正确的

