---
title: Hexo(六)：绑定个人域名
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 绑定个人域名

现在使用的域名是Github提供的二级域名，也可以绑定为自己的个性域名。购买域名，可以到[GoDaddy官网](https://link.jianshu.com/?t=https%253A%252F%252Fsg.godaddy.com%252Fzh%252F)，，也可以到[阿里万网](https://link.jianshu.com/?t=http%253A%252F%252Fwanwang.aliyun.com%252F)购买。

### 1.Github端

在/blog/themes/landscape/source目录下新建文件名为：CNAME文件，注意没有后缀名！直接将自己的域名如：victoryofymk.com写入。

终端cd到blog目录下执行如下命令重新部署：

```

$ hexo clean

$ hexo g

$ hexo d

```

注意：网上许多都是说在Github上直接新建CNAME文件，如果这样的话，在你下一次执行hexo d部署命令后CNAME文件就消失了，因为本地没有此文件。

### 2.域名解析

如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录。登录万网，在你购买的域名后边点击：解析 --\> 添加解析
```
记录类型：CNAME
主机记录：将域名解析为[example.com](https://link.jianshu.com/?t=http%253A%252F%252Fexample.com)（不带www），填写@或者不填写
记录值：victoryofymk.github.io. (victoryofymk改为你自己的用户名)，点击保存即可
域名解析
```

此时，点击访问[http://victoryofymk.com](https://link.jianshu.com/?t=http%253A%252F%252Fgonghonglou.com)和访问[http://victoryofymk.github.io](https://link.jianshu.com/?t=http%253A%252F%252Fgonghonglou.github.io)效果一致，大功告成！

参考的文章：
  1.http://gonghonglou.com/2016/02/03/firstblog/

