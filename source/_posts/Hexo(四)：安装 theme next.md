---
title: Hexo(四)：安装 theme next
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
tags:
- hexo #文章標籤 可以省略
- blog
---

## 安装theme
你可以到Hexo官网主题页去搜寻自己喜欢的theme。这里以hexo-theme-next为例

终端cd到 blog 目录下执行如下命令：
```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
将blog目录下_config.yml里theme的名称landscape修改为next
终端cd到blog目录下执行如下命令(每次部署文章的步骤)：
```
$ hexo clean           //清除缓存文件 (db.json) 和已生成的静态文件 (public)
$ hexo g             //生成缓存和静态文件
$ hexo d             //重新部署到服务器
```
至于更改theme内容，比如名称，描述，头像等去修改blog/_config.yml文件和blog/themes/next/_config.yml文件中对应的属性名称即可， 不要忘记冒号:后加空格。 NexT 使用文档里有极详细的介绍。

升级 nexT 主题，cd 到 blog/themes/next/ 下执行命令 git pull 更新

### 设置语言

NexT 目前支持六种语言版本：
	• English
	• 中文简体 (zh-Hans)
	• French (fr-FR)
	• 正体中文 (zh-hk/zh-tw)
	• Russian (ru)
	• German (de)
默认语言是英文。编辑站点的_config.yml，将language字段更改为你所需要的语言版本代号：
language: default #默认是英文
```
language: zh-CN
language: fr-FR
language: zh-HK
language: zh-TW
language: en
language: ru
language: de
```
### 设置文章目录
### Toc true
### 设置归档

