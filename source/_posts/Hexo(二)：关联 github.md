---
title: Hexo(二)：关联 github
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
tags:
- hexo #文章標籤 可以省略
- blog
---

##  创建仓库
登录你的Github帐号，新建仓库，名为用户名.github.io固定写法，如victoryofymk.github.io

## 修改blog配置
本地的blog文件夹下内容为：
```
_config.yml 
db.json 
node_modules 
package.json
scaffolds
source
themes

```
终端cd到blog文件夹下，vim打开_config.yml，命令如下：
```
$ vim _config.yml
```
打开后往下滑到最后，修改成下边的样子：
```
deploy:
  type: git
  repository: https://github.com/victoryofymk/victoryofymk.github.io.git
   branch: master     

```
将repository后victoryofymk换成你自己的用户名

注意：在配置所有的_config.yml文件时（包括theme中的），在所有的冒号:后边都要加一个空格，否则执行hexo命令会报错

则执行hexo deploy命令时终端会提示你输入Github的用户名和密码，即
```
Username for 'xxx':
Password for 'xxx':
```
hexo deploy命令执行成功后，浏览器中打开网址http://victoryofymk.github.io（将victoryofymk换成你的用户名）能看到和打开http://localhost:4000时一样的页面。

**注意**：若执行命令hexo deploy仍然报错：无法连接git或找不到git，则执行如下命令来安装hexo-deployer-git：
```
$ npm install hexo-deployer-git --save
```
再次执行hexo deploy命令。

未避免每次输入Github用户名和密码的麻烦，可参照下一节
## 添加ssh key到Github
1.检查SSH keys是否存在Github
执行如下命令，检查SSH keys是否存在。如果有文件id_rsa.pub或id_dsa.pub，则直接进入步骤3将SSH key添加到Github中，否则进入下一步生成SSH key。
```
$ ls -al ~/.ssh
```
2.生成新的ssh key
执行如下命令生成public/private rsa key pair，注意将your_email@example.com换成你自己注册Github的邮箱地址。
```
$ ssh-keygen -t rsa -C "your_email@example.com"
```
默认会在相应路径下（~/.ssh/id_rsa.pub）生成id_rsa和id_rsa.pub两个文件。
3.将ssh key添加到Github中
Find前往文件夹~/.ssh/id_rsa.pub打开id_rsa.pub文件，里面的信息即为SSH key，将这些信息复制到Github的Add SSH key页面即可。
```
进入Github --> Settings --> SSH keys --> add SSH key:
```
Title里任意添一个标题，将复制的内容粘贴到Key里，点击下方Add key绿色按钮即可。
## 将个人博客同时部署到 GitHub 和 Coding

1、首先到 Coding 上注册并开一个项目，项目名称和用户个性后缀相同（方便二级域名访问博客），拿到项目的 https 地址
2、打开本地 blog 目录下的 _config.yml 文件，修改如下
```
deploy:
  type: git
  repository: 
            github: https://github.com/xxx/xxx.github.io.git
            coding: https://git.coding.net/xxx/xxx.git
  branch: master
```
3、cd 到本地 blog/source 目录下执行如下命令新建 Staticfile 文件
```
$ touch Staticfile  #名字必须是Staticfile
```
原因是 coding.net 需要以这个文件来作为静态文件部署的标志，就是说看到这个 Staticfile 就知道按照静态文件来发布。
4、执行发布命令 
```
hexo g 、 hexo d
```
5、个人域名添加两条 CNAME 解析。将 xxx.github.io. 解析为 [海外] ，将 xxx.coding.me. 解析为 [默认]
这样就是为了从国内访问 xxx.com 就是访问 Coding 上的博客项目，从国外访问 xxx.com 就是访问 GitHub 上的博客项目。
6、到 Coding 上的博客项目主页，点击 Pages服务 输入部署分支 master 立即开启

这样就可以访问自己在 Coding 上的个人博客了

