---
title: VUE(七)：vue使用中的问题
date: 2018-10-29 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## 常见问题
### i-table的render方法this指向window

i-table 中render渲染的方法this指向window

解决办法:定义VUE时使用变量接收
```
let vue=new VUE（｛｝）
```

### 淘宝镜像
```
ERROR command failed: npm install --loglevel error --registry=https://registry.npm.taobao.org --disturl=https://npm.taobao.org/dist
```
编辑.vuerc，修改配置"useTaobaoRegistry": false
```
vi ~/.vuerc
```

