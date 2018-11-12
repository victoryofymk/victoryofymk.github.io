---
title: Hexo(十)：使用中的问题
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 常见问题
### Template render error
解析异常，通常在hexo g的时候出现，通常是某些代码被解析

```
Template render error: (unknown path)
  Error: template not found: ./comments/livere.swig
```
如果让jekyll不解析，使用raw语法，中间写内容

```

{% raw %}

{% sometag %}

{% endraw %}

```
### Found incompatible module

使用yarn管理依赖

```
error nunjucks@3.1.3: The engine "node" is incompatible with this module. Expected version ">= 6.9.0 <= 11.0.0-0". Got "11.0.0"
```

需要将node降级为7，在travis.yml中修改

```
# node版本
node_js: "7"
```

### gitalk授权问题

使用github issue实现hexo评论功能，登陆时无法跳转到有效链接

解决办法：检查MD文件名称中是否有中文字符，例如：中文冒号：，如果有去掉即可，因为跳转时会转换成英文字符，所以路径就找不到，空格也可能有问题

