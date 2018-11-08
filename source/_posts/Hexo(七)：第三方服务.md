---
title: Hexo(七)：第三方服务
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
comments: true
tags:
- hexo #文章標籤 可以省略
- blog
---

## 评论系统

### 添加disqus评论
由于以前国内使用较多的多说评论下架了，所以选用了国外较为稳定的disqus，但使用该评论功能需要“科学上网”！

	• 注册disqus账号https://disqus.com
	• 在disqus设置页面中点 Add Disqus to your site 添加你的网站地址(即为https://yourname.github.io), 和设置Choose your unique Disqus URL, 你所填写的unique Disqus URL即为hexo配置文件中需要修改的short_name字段。
	• 打开hexo/themes/next/_config.yml主题配置文件，修改下面字段：
	
```
#Disqus
disqus:
  enable: true
  shortname:     #shortname即为你上面填写的唯一disqus路径，填上就好
  count: true
```
### 添加Hypercomments
来自俄罗斯,提供付费和免费的服务
### 添加Valine
基于LeanCloud，也支持阅读量等，免费版有一定限制首先API请求每天30000，存储10G

修改next主题配置文件

```
# Valine.
# You can get your appid and appkey from https://leancloud.cn
# more info please open https://valine.js.org
valine:
  enable: false # When enable is set to be true, leancloud_visitors is recommended to be closed for the re-initialization problem within different leancloud adk version.
  appid:  # your leancloud application appid
  appkey:  # your leancloud application appkey
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
  visitor: false # leancloud-counter-security is not supported for now. When visitor is set to be true, appid and appkey are recommended to be the same as leancloud_visitors' for counter compatibility. Article reading statistic https://valine.js.org/visitor.html

```

### 添加Gitment
基于github的issue实现的,不再维护
### 添加Gitalk
基于github的issue实现的,有人维护

登陆GitHub，然后点击头像，然后Settings，Developer settings,然后，点击OAuth Apps，New OAuth App创建
```
参数说明：
Application name： # 应用名称，随意
Homepage URL： # 网站URL，如https://asdfv1929.github.io
Application description # 描述，随意
Authorization callback URL：# 网站URL，https://asdfv1929.github.io
```
创建完成后client id和client secret在配置文件中需要使用

修改next主题配置，新增

```
# Gitalk评论
gitalk:
  enable: true
  owner: #这个项目名的拥有者（GitHub账号或组织）
  repo: #你要存放的项目名
  admin: #这个项目名的拥有者（GitHub账号或组织）
  labels: gitalk #GitHub issues的标签，下面会详细说
  clientID: #client id
  clientSecret: #client secret
  gitalk_css: //cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css 
  gitalk_js: //cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js #
  md5: //cdn.bootcss.com/blueimp-md5/1.1.0/js/md5.min.js
```

gitalk.swig如果已经配置css和js可以省略，md5加密ID可以解决gitbub对Issue label长度最多50的限制

#### 配置主题方法一
找到NexT的主题目录，然后进入这个路径/next/layout/_custom/下，应该有head.swig，header.swig，sidebar.swig这三个文件。
这三个文件应该就是自定义布局的位置。然后，在sidebar.swig里面添加如下代码：
```
{% if page.comments and config.gitalk.enable %}
  <link rel="stylesheet" href="{{ config.gitalk.gitalk_css }}">
  <script src="{{ config.gitalk.gitalk_js }}"></script>
  <script src="{{ config.gitalk.md5 }}"></script>
  <script>
      var gitalk = new Gitalk({
        clientID: '{{ config.gitalk.clientID }}',
        clientSecret: '{{ config.gitalk.clientSecret }}',
        repo: '{{ config.gitalk.repo }}',
        owner: '{{ config.gitalk.owner }}',
        admin: '{{ config.gitalk.admin }}',
        id: md5(location.pathname),
        distractionFreeMode: 'true'
      });
      var div = document.createElement('div');
      div.setAttribute("id", "gitalk_comments");
      div.setAttribute("class", "post-nav");
      var bro = document.getElementById('posts').getElementsByTagName('article');
      bro = bro[0].getElementsByClassName('post-block');
      bro = bro[0].getElementsByTagName('footer');
      bro = bro[0];
      bro.appendChild(div);
      gitalk.render('gitalk_comments');
  </script>
{% endif %}
```
参考：https://yunhao.space/2018/07/04/hexo-next-gitalk-comments-tutor/
#### 配置主题方法二

以NexT主题做示范，毕竟其他的也是大同小异。
在主题的\layout\_third-party\comments目录中，新建一个gitalk.swig文件，文件内容如下：
```
{% if page.comments && theme.gitalk.enable %}
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
  <script src="{{ theme.gitalk.md5 }}"></script>
   <script type="text/javascript">
        var gitalk = new Gitalk({
          clientID: '{{ theme.gitalk.ClientID }}',
          clientSecret: '{{ theme.gitalk.ClientSecret }}',
          repo: '{{ theme.gitalk.repo }}',
          owner: '{{ theme.gitalk.owner }}',
          admin: ['{{ theme.gitalk.admin }}'],
          id: md5(location.pathname),
          distractionFreeMode: '{{ theme.gitalk.distractionFreeMode }}'
        })
        gitalk.render('gitalk-container')
       </script>
{% endif %}
```
还是，在主题的\layout\_third-party\comments目录中，找到一个index.swig的文件，打开，添加这一行代码：
```
{% include 'gitalk.swig' %}
```
接着，在主题的\layout\_partials目录中，找到comments.swig文件，打开，找到

```
{% elseif theme.valine.appid and theme.valine.appkey %}
  <div class="comments" id="comments">
  </div>  

{% endif %}
```
在空了一行的地方加上以下代码：
```
{% elseif theme.gitalk.enable %}
<div id="gitalk-container"></div>
```

新建/source/css/_common/components/third-party/gitalk.styl文件，添加内容：

```
.gt-header a, .gt-comments a, .gt-popup a
  border-bottom: none;
.gt-container .gt-popup .gt-action.is--active:before
  top: 0.7em;
```

修改/source/css/_common/components/third-party/third-party.styl，在最后一行上添加内容，引入样式：

```
@import "gitalk";
```

## 添加百度分享功能
百度分享功能的添加可以参考下面这篇博客。[Hexo+Github搭建个人博客(三)——百度分享集成](http://blog.csdn.net/cl534854121/article/details/76121105)
## 百度统计访客访问量功能
其他酷炫小功能参考[hexo的next主题个性化教程:打造炫酷网站](http://www.jianshu.com/p/f054333ac9e6)。

