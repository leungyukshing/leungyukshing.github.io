---
title: 为Hexo博客添加自动化部署Travis
abbrlink: 36850
date: 2019-08-20 01:42:22
tags:
---

## Intro

&emsp;&emsp;我打理自己的博客也有快两年了，期间做过不少的优化，最近希望对整个博客做一个比较大的提升——添加自动化部署。这里用到的工具是[travis-ci](<https://travis-ci.org/>)，它是一种持续集成的工具，它提供了一个持续集成功能平台，能够为我的博客项目配置好需要执行的操作，如编译、测试、打包、部署等操作。在这篇博客我就来和大家分享一下我的升级之路。

<!-- more -->

---

## 什么是CI

&emsp;&emsp;CI(Continuous Integration)，持续集成，是一种软件实践的过程。通俗来讲就是我们多次地把代码提交，然后对产品不断打包和发布。常用的构建工具有：

+ [Jenkins](<https://jenkins.io/>)
+ [Travis](<https://travis-ci.org/>)
+ [Codeship](<https://codeship.com/>)

&emsp;&emsp;Jenkins在工业界用的比较多，但是它仅支持在内网进行CI，所以对于我们的开源博客来说，比较合适的还是Travis。

---

## 为什么要CI

&emsp;&emsp;到这里，可能你还是一脸懵逼，不知道为什么要搞这么麻烦。接下来我结合hexo的特性来分析一下我们添加Travis-ci的必要性。

&emsp;&emsp;首先基于hexo的个人博客发布博文无非就是三板斧：

+ `hexo new`
+ `hexo generate`
+ `hexo deploy`

&emsp;&emsp;相信各位hexo的老手们对这三条命令是非常的熟悉，但是，我们知道，github仓库上仅仅只会保留部署后生成的html和一些主题配置文件用于渲染GitHub Page，而我们写的md文件是没有保存的。为了解决这个问题，我是用的分支的解决办法，通过新建一个`hexo`分支，将我们手写的文件备份（具体可以看这里[传送门](<http://leungyukshing.cn/archives/BlogBackup.html>)）。

&emsp;&emsp;这样子做法解决了我们文件备份的问题，但是麻烦的一点是，每一次我们部署和备份文件是分开的，即我们部署的时候是更新`master`分支，而备份则需要更新`hexo`分支，这种开发效率对我来说当然是不能忍受的。

&emsp;&emsp;除了基于开发效率的考虑外，还有一个痛点就是项目迁移问题。如果我们在其他机器上想更新我们的博客，我们需要重新配置所有的环境，包括nodejs和hexo等，这个是非常麻烦的。

&emsp;&emsp;这个时候CI就能够很好地帮助我们解决这两个问题。CI的任务可以理解为：每当我们提交一次源文件（md文件），它就将自动地帮我们把整个项目重新构建一次，这样我们的工作就简化为只需要提交我们写的博文了。

&emsp;&emsp;CI的工作原理可以简单想象为：我们定义了一系列的操作，而我们的`git push`操作将会激发CI去把我们定义的操作都执行一边，这就是自动化部署。

---

## 配置过程

### 注册Travis账号

首先到Travis的官网注册账号，注意：要使用Github账号登陆，否则可能关联不到GitHub的项目。

![注册Travis账号](/images/travis0.png)

进入之后就能看到GitHub的所有仓库，我们选择博客的仓库，点击Setting。

![进入setting](/images/travis1.png)

### 配置CI信息

这一步的作用主要是开启该项目的CI，这样只要我们提交到GitHub中，CI就会自动启动。同时我们还可以设置环境变量。虽然环境变量也可以在后续的yml文件中设置，但是因为yml文件是公开的，所以一些有关权限的token和个人信息可以在这里设置，避免信息泄露。

![开启CI](/images/travis2.png)

然后是配置环境变量。可以看到这里我有两个TOKEN，这两个TOKEN的作用是验证对仓库的权限，第一个`CO_TOKEN`是coding.net仓库的token（这是我用于镜像的仓库，可以忽略）。第二个`REPO_TOKEN`是GitHub仓库的token，我们需要在github中获取。

![CI环境变量](/images/travis3.png)

获取GitHub的token，首先进入个人设置（注意是个人头像下的setting，而不是某个仓库的setting），进入`Developer settings->Personal access tokens`。然后点击右上方的`Generate new token`，权限选择`repo`即可（给予过多的权限可能会造成安全问题）。创建成功后我们就能看到token内容，注意保存，它只会出现一次，忘记了只能重新创建了（这个token最好不要以明文的形式保存，因为拥有这个token就能够对你的仓库进行很多操作）。最后我们把这个token填回到Travis那里就可以了。

![获取Github仓库token](/images/travis4.png)

### 配置构建脚本

这一步我们将编写一个yml格式的文件，用于给CI自动部署。我们要在博客的根目录下创建一个文件`.travis.yml`，里面编写的语法需要符合travis的规范，这里我直接给出。

```yml
# 使用语言为 Node.js
language: node_js
# Node.js 版本，Node 11.0.0 版本 yarn install 会报错不支持 
# node_js: stable
node_js: 10.10.0
# 设置只监听哪个分支 
branches:
  only:
  - hexo
# 缓存，可以节省集成的时间，这里用了 yarn, 如果不用可以删除 
cache:
  apt: true
  yarn: true
  directories:
    - node_modules
    - CNAME
# env
# 全局变量，为了安全，不要在这里设置，我这里设置只是示例，其实没有用到 
env:
 global:
   - GITHUB_URL: github.com/leungyukshing/leungyukshing.github.io.git
   - CODING_URL: git.coding.net/leungyukshing/leungyukshing.coding.me.git
# tarvis 生命周期执行顺序详见官网文档 
before_install:
# 更改时区 
- export TZ='Asia/Shanghai'
- git config --global user.name ${USER_NAME}
- git config --global user.email ${USER_EMAIL}
# 由于使用了 yarn, 所以需要下载，不用 yarn 这两行可以删除 
#- curl -o- -L https://yarnpkg.com/install.sh | bash
#- export PATH=$HOME/.yarn/bin:$PATH
# hexo 基础工具 
- npm install -g hexo-cli
# 初始化所需模块，在 package.json 中配置的有，这样 node_modules 就不用提交到 GitHub 了 
# 下次如果换电脑，安装完 Node.js, 全局 (-g 参数) 安装完 hexo-cli, 直接在项目根目录初始化即可 (在 package.json 配置的都会自动下载)
- npm install
# 本地搜索需要工具 
- npm install hexo-generator-searchdb --save
# 字数统计，时长统计需要工具，Node 版本 7.6.0 之前，请安装 2.x 版本，npm install hexo-wordcount@2 --save
# - npm install hexo-wordcount --save
# 站点地图，seo 优化使用 
- npm install hexo-generator-sitemap --save
- npm install hexo-generator-baidu-sitemap --save
# 中英文之间自动增加空格插件 
# - npm install hexo-filter-auto-spacing --save (这个人用的少，不用了)
- npm install hexo-pangu-spacing --save
# rss 订阅插件 
- npm install hexo-generator-feed --save
# 三维卡通人物 
# - npm install --save hexo-helper-live2d
# 三维卡通人物 - 小猫咪模型下载 (保险起见，把模型文件放到自己的文件夹里，不用每次下载)
# - npm install --save live2d-widget-model-hijiki
# 压缩文件 
- npm install hexo-neat --save
install:
# 不用 yarn 的话这里改成 npm i 即可 
- npm install
script:
- hexo clean
- hexo generate
# 成功之后才会提交，如果失败就会跳过并发送失败邮件通知 
after_success:
# 获取 master 分支内容 
- git clone https://${GITHUB_URL} .deploy_git
- cd .deploy_git
- git checkout master
- cp -rf ../public/* ./
# 必要配置文件 
- cp -rf ../README.md ../.gitignore ../CNAME ../index.php ./
# 更改 .gitignore 内容，与 source 分支的不一样，直接置空，目前没有需要过滤的 
- echo "" > ./.gitignore
- git add .
# 提交记录包含时间，跟上面更改时区配合 
- git commit -m "Travis CI Auto Builder at `date +"% Y-% m-% d % H:% M"`"
# 推送到主分支 
- git push --force --quiet "https://${REPO_TOKEN}@${GITHUB_URL}" master:master
# Mirror Repo
- git push --force --quiet "https://leungyukshing:${CO_TOKEN}@{CODING_URL}" master:master
# 邮件通知机制，我在这里设置了成功 / 失败都会通知，这里不能使用环境变量 
# configure notifications (email, IRC, campfire etc)
# please update this section to your needs!
# https://docs.travis-ci.com/user/notifications/
notifications:
  email:
    - jacky14.liang@gmail.com
  on_success: always
  on_failure: always
```

大致分析一下结构：

+ `language`：本项目的主要语言，hexo博客的话就是基于nodejs。
+ `node_js`：nodejs版本号。
+ `branches`：CI监听的分支，这里我的md文件是保存在`hexo`分支，所以监听的也就是`hexo`。
+ `cache`：缓存部分，可以节省集成时间。
+ `env`：这里也是前面在配置CI信息提到过的环境变量，一些不太敏感的信息可以写在这里。如用户名、仓库路径等。
+ `before_install`：构建前的环境搭建。这里需要安装所有依赖，包括博客用到的所有插件，这一部分大家需要按需修改，一般格式就是`npm install [对应依赖]`
+ `install`：npm包安装。
+ `script`：在安装完后会执行的脚本文件，这里我们就写部署的代码。每次部署都需要先清除再部署，虽然看上去好像有点浪费时间，但这是软件工程开发的规范。
+ `after_success`：这里定义的是构建成功后的行为。构建成功后我们把代码提交到`master`分支，给Github Page渲染。
+ `notification`：通知提醒。我们每次构建完后都可以设置邮件提醒，这里我设置的是成功与失败都发送提醒。

至此，CI的整个配置就完成了。接下来我们每次写博客的步骤简化为：

+ `hexo new [post name]` 
+ `git add .`
+ `git commit -m "commit message"`
+ `git push origin hexo`

这四条命令能够部署和备份我们的文件，非常方便。我们每次push后，可以到travis的网站看到我们的项目构建。

![项目构建](/images/travis5.png)

## 总结

&emsp;&emsp;希望通过这篇博客能够与大家分享为开源项目添加CI的过程，开发的效率确实提高了不少，当然我自己也在探索当中，比如说能不能把CI的链接在每次push后打印到命令行中。希望这篇博客能够帮助到你，谢谢！