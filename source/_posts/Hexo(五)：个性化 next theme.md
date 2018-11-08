# ---
title: Hexo(五)：个性化 next theme
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 个性化
主要有以下32种：
	• 在右上角或者左上角实现fork me on github
	• 添加RSS
	• 添加动态背景
	• 实现点击出现桃心效果
	• 修改文章内链接文本样式
	• 修改文章底部的那个带#号的标签
	• 在每篇文章末尾统一添加“本文结束”标记
	• 修改作者头像并旋转
	• 博文压缩
	• 修改``代码块自定义样式
	• 侧边栏社交小图标设置
	• 主页文章添加阴影效果
	• 在网站底部加上访问量
	• 添加热度
	• 网站底部字数统计
	• 添加 README.md 文件
	• 设置网站的图标Favicon
	• 实现统计功能
	• 添加顶部加载条
	• 在文章底部增加版权信息
	• 添加网易云跟帖(跟帖关闭，已失效，改为来必力)
	• 隐藏网页底部powered By Hexo / 强力驱动
	• 修改网页底部的桃心
	• 文章加密访问
	• 添加jiathis分享
	• 博文置顶
	• 修改字体大小
	• 修改打赏字体不闪动
	• 自定义鼠标样式
	• 为博客加上萌萌的宠物
	• DaoVoice 在线联系
	• 点击爆炸效果
	
	下面介绍几种常见配置,其他请参考文末链接
	
### 1.在右上角或者左上角实现fork me on github

点击这里https://blog.github.com/2008-12-19-github-ribbons/挑选自己喜欢的样式，并复制代码。 例如，我的代码：
```
<a href="https://github.com/Ahaochan" class="" target="_blank" title="我的Github" aria-label="我的Github"><svg width="80" height="80" viewBox="0 0 250 250" style="fill:#222; color:#fff; position: absolute; top: 0; border: 0; right: 0;" aria-hidden="true"><path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path><path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path><path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z" fill="currentColor" class="octo-body"></path></svg>
  </a>
```

然后粘贴刚才复制的代码到themes/next/layout/_layout.swig文件中(放在<div class="headband"></div>的下面)，并把href改为你的github地址


### 2.添加RSS

执行以下命令安装 RSS 插件:

```
$ npm install hexo-generator-feed --save
```

编辑网站根目录下的 _config.yml，添加以下代码开启

```
# RSS订阅支持
plugin:
- hexo-generator-feed

# Feed Atom
feed:
type: atom
path: atom.xml
limit: 20
```

NexT 主题，默认就可以；其他主题请参考主题说明，配置完之后运行：

```
$ hexo clean && hexo g
```

重新生成一次，你会在./public 文件夹中看到 atom.xml 文件。然后启动服务器查看是否有效，之后再部署到 Github 中。

### 3. 侧边栏社交小图标设置

打开主题配置文件（_config.yml），配置social

```
social:
  #GitHub: https://github.com/yourname || github
  GitHub: https://github.com/victoryofymk || github
  #E-Mail: mailto:yourname@gmail.com || envelope
  E-Mail: mailto:starlord.yan@gmail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons:
  enable: true
  icons_only: false
  transition: false
  # Dependencies: exturl: true in Tags Settings section below.
  # To encrypt links above use https://www.base64encode.org
  # Example encoded link: `GitHub: aHR0cHM6Ly9naXRodWIuY29tL3RoZW1lLW5leHQ= || github`
  exturl: false
```

### 4. 添加热度

### 5. 文章字数统计和阅读时间估计

运行  

```
npm install hexo-symbols-count-time --save
```
NexT 主题也对这个插件进行了集成，可以进行一些高级设置

```
# 「在NexT配置文件里修改」

symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: false
  awl: 4 # Average Word Length
  wpm: 275 # Words Per Minute
```
### 6. 根据标签推荐相关文章
运行

```
npm install hexo-related-popular-posts --save
```

 NexT 主题集成了这个插件的配置
 
```
# 「在NexT配置文件里修改」

# Related popular posts
# Dependencies: https://github.com/tea3/hexo-related-popular-posts
related_posts:
  enable: true
  title: # custom header, leave empty to use the default one
  display_in_home: false
  params:
    maxCount: 5
    #PPMixingRate: 0.0
    #isDate: false
    #isImage: false
    #isExcerpt: false
```
### 7. 设置阅读访问量

关于Hexo博客的文章阅读量设置问题，常见方案如下：
1、不蒜子，仅局限于在文章页面显示阅读数，首页是不显示的。

打开主题配置文件（_config.yml）
```
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: user
  total_views: true
  total_views_icon: eye
  post_views: true
  post_views_icon: eye
```
2、LeanCloud
打开LeanCloud官网，进入注册页面注册。完成邮箱激活后，点击头像，进入控制台页面，如下：

#### 创建应用
创建应用取名test

#### 创建Class
创建Class命名为Counter,ACL权限默认即可

#### Next主题配置
基本配置
编辑主题目录下的site_page/themes/next/_config.yml配置，修改配置如下：

```
# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: xxx
  app_key: xxx
```

其中，app_id和app_key在设置–应用Key中获取
#### Web安全
为了保证应用的统计计数功能仅应用于自己的博客系统，你可以在应用->设置->安全中心的Web安全域名中加入自己的博客域名，以保证数据的调用安全。
#### 确认其他配置
```
themes\next\layout\_scripts\third-party\lean-analytics.swig  # 确保该文件存在
themes\next\layout\_macro\post.swig                          # 大约171行存在leancloud_visitors相关代码
themes\next\layout\_layout.swig                              # 大约83行引用了lean-analytics.swig文件
```
#### 问题

报错：Cannot read property ‘enable_sync’ of undefined
解决：在blog站点下的配置文件_config.yml添加以下代码

```
leancloud_counter_security:
  enable_sync: true
  app_id: rjEGiCa******-gzGzoHsz
  app_key: zWE2Bry******8HmPyTpBQa
  username: # Will be asked while deploying if is left blank
  password: # Recommmended to be left blank. Will be asked while deploying if is left blank
```

### 8. 添加sitemap和baidusitemap
安装sitemap

```
npm install hexo-generator-sitemap --save
```

安装baidusitemap

```
npm install hexo-generator-baidu-sitemap --save
```

在博客目录的_config.yml中添加如下代码,并修改url字段的值，其值默认为http://yoursite.com

```
# 自动生成sitemap
sitemap: 
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml

url: https://victoryofymk.github.io
```
### 9.为博客加上萌萌的宠物

安装：
```
npm install -save hexo-helper-live2d
```

请向Hexo的 _config.yml 文件或主题的 _config.yml 文件中添加配置.
```
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-wanko
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```
然后hexo clean ，hexo g ，hexo d 就可以看到了。

### 10.开启动态背景
安装

```
git clone https://github.com/theme-next/theme-next-canvas-nest themes/next/source/lib/canvas-nest
```


参考的文章：
	1. https://segmentfault.com/a/1190000009544924


