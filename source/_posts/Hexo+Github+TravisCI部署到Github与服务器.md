---
title: Hexo+Github+TravisCI部署到Github 与 自己的服务器
date: 2018-10-23 16:12:48
categories: "hexo" #文章分類目錄 可以省略
tags:
- hexo #文章標籤 可以省略
- blog
---

## 自动部署
使用hexo在github上搭blog最大的问题就是，每次提交都需要先hexo -g，然后再deploy生成的文件们，这样哪怕是改一个小的地方都需要重新编译全部blog，因此使用Travis来自动持续集成提交到github以后的操作，具体逻辑：
  * 写完blog后，直接push到github的source分支，其它的就可以不用管了
  * 由于我的.travis.yml配置文件里设置监听的就是source分支，所以会触发webhook
  * Travis则会将该项目clone过去，然后按照.travis.yml的设置执行接下来的命令
  * 执行完成后，再将编译好的文件们发送到自己的服务器，顺便push回master分支上来
  * 这样就可以在blog.godi13.com和Godi13.github.io上都访问blog了

### Github
为了使travis能够将编译好的文件们push回github，我们需要生成token，步骤如下：
1) 点击github右上方头像，然后点setting，或者https://github.com/settings/profile
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023150449.png)
2) 点击Developer settings
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023154052.png)
3) 进入Personal access tokens，点击Generate new token,为token起一个名字，勾选repo，然后点击生成,生成token以后，一定要复制好，因为只显示一次，如果丢失只能再次生成
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023150618.png)

### Travis
1) 使用github帐号登录Travis，右上方按钮点击同步项目，下方打开需要集成的项目，最后点击齿轮进入项目配置页面
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023154227.png)
2) 勾选相应配置，然后往下移动页面到环境变量
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023150236.png)
3) 在这里我将变量名称名为REPO_TOKEN，放上token，点击Add按钮
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023145955.png)

### Terminal
回到终端，进入blog所在的文件夹下，新建.travis.yml文件，并添加以下内容
```
# 使用语言
language: node_js
# node版本
node_js: stable
# 设置只监听哪个分支
branches:
  only:
  - source
# 缓存，可以节省集成的时间，这里我用了yarn，如果不用可以删除
cache:
  apt: true
  yarn: true
  directories:
    - node_modules
# tarvis生命周期执行顺序详见官网文档
before_install:
- git config --global user.name "Godi13"
- git config --global user.email "mqzq9388@gmail.com"
# 由于使用了yarn，所以需要下载，如不用yarn这两行可以删除
- curl -o- -L https://yarnpkg.com/install.sh | bash
- export PATH=$HOME/.yarn/bin:$PATH
- npm install -g hexo-cli
install:
# 不用yarn的话这里改成 npm i 即可
- yarn
script:
- hexo clean
- hexo generate
after_success:
- cd ./public
- git init
- git add --all .
- git commit -m "Travis CI Auto Builder"
# 这里的 REPO_TOKEN 即之前在 travis 项目的环境变量里添加的
- git push --quiet --force https://$REPO_TOKEN@github.com/Godi13/Godi13.github.io.git
  master
```

然后，准备push该项目到github，看下是否成功，如果是新项目可参照下面的git指令
```
git init
# 添加自己的项目
git remote add origin git@github.com:Godi13/Godi13.github.io.git
# 新建并切换分支
git checkout --orphan source
git add -A
git commit -m "Travis CI"
git push
```
关于 --orphan 请参考[如何建立一個沒有 Parent 的獨立 Git branch](https://ihower.tw/blog/archives/5691)

如最终成功则会看到
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181023145751.png)

## 服务器
未完…

参考资料
[使用 Travis 自动部署 Hexo 到 Github 与 自己的服务器](https://segmentfault.com/a/1190000009054888)
[Hexo作者的.travis.yml配置](https://github.com/tommy351/tommy351.github.io/blob/source/.travis.yml)
[用 Travis CI 自動部署網站到 GitHub](https://zespia.tw/blog/2015/01/21/continuous-deployment-to-github-with-travis/)
[一点都不高大上，手把手教你使用Travis CI实现持续部署](https://zhuanlan.zhihu.com/p/25066056)
[使用 Travis CI 自动更新 GitHub Pages](http://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)
[使用Travis CI自动构建hexo博客](http://magicse7en.github.io/2016/03/27/travis-ci-auto-deploy-hexo-github/)










  

