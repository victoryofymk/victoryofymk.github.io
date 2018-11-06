---
title: Hexo(七)：第三方服务
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
tags:
- hexo #文章標籤 可以省略
- blog
---

## 多说评论系统
** 注意 **：duoshuo_shortname不是你的多说登录的 id
使用多说前需要先在 多说 创建一个站点。具体步骤如下： 
		1. 登录后在首页选择 “我要安装”。
		2. 创建站点，填写站点相关信息。多说域名 这一栏填写的即是你的duoshuo_shortname，如图：
		多说设置示例：
duoshuo_shortname: iissnan-notes
	• 百度统计
	• Swiftype搜索

## 添加disqus评论
由于以前国内使用较多的多说评论下架了，所以选用了国外较为稳定的disqus，但使用该评论功能需要“科学上网”！
	• 注册disqus账号https://disqus.com
	• 在disqus设置页面中点 Add Disqus to your site 添加你的网站地址(即为https://yourname.github.io), 和设置Choose your unique Disqus URL, 你所填写的unique Disqus URL即为hexo配置文件中需要修改的short_name字段。
	• 打开hexo/themes/next/_config.yml主题配置文件，修改下面字段：
#Disqus
disqus:
  enable: true
  shortname:     #shortname即为你上面填写的唯一disqus路径，填上就好
  count: true
## 添加百度分享功能
百度分享功能的添加可以参考下面这篇博客。[Hexo+Github搭建个人博客(三)——百度分享集成](http://blog.csdn.net/cl534854121/article/details/76121105)
## 百度统计访客访问量功能
其他酷炫小功能参考[hexo的next主题个性化教程:打造炫酷网站](http://www.jianshu.com/p/f054333ac9e6)。

