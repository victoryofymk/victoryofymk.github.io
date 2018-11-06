---
title: Hexo(三)：创建分类页面
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
tags:
- hexo #文章標籤 可以省略
- blog
---

## 创建分类

添加一个 分类 页面，并在菜单中显示页面链接。

1. 新建一个页面，命名为 categories 。命令如下：
```
hexo new page categories
```
2. 编辑刚新建的页面，将页面的类型设置为 categories ，主题将自动为这个页面显示所有分类。
 ``` 
 title: 分类
 date: 2014-12-22 12:39:04
 type: "categories"
 ---
 ```

 注意：如果有启用多说 或者 Disqus 评论，默认页面也会带有评论。需要关闭的话，请添加字段 comments 并将值设置为 false，如：
 ```
 title: 分类
 date: 2014-12-22 12:39:04
 type: "categories"
 comments: false
 ---
 ```
3. 在菜单中添加链接。编辑主题的 \_config.yml ，将 menu 中的 categories: /categories注释去掉，如下:
 ``` 
 menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
 ```

