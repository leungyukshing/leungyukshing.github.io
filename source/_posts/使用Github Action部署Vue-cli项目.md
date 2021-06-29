---
title: 使用Github Action部署Vue-cli项目到Github Pages
abbrlink: 25800
date: 2020-01-03 14:52:23
tags:
---

## 介绍

&emsp;&emsp;最近在做一个项目，前端使用Vue-cli搭建，想把静态的页面部署到GitHub Pages上展示。之前我的[个人博客](http://leungyukshing.cn/)使用的是hexo的部署模式，结合Travis CI实现了自动部署（[传送门](http://leungyukshing.cn/archives/Hexo-Travis.html)）。但是整个配置还是比较麻烦的，部署分支需要自己管理，编写`travis.yml`也花费了大量的时间。这次我想换一种简单直接的方法，把静态页面部署到Github Pages上。

<!-- more -->

---

## Github Action

&emsp;&emsp;Github Action推出也有一段时间了，但是仍然没有受到大众的关注，网上的教程不多，多数人还是采用Travis的CI方式。实际上，它提供的CI与Travis相差无几，而在兼容性上更加是优于Travis。最令人满意的一点是，许多大佬开源了Github Action的脚本，让我们配置CI的过程变得非常容易。

&emsp;&emsp;在这个项目中，我将使用Github Action对前端项目进行测试、构建以及自动部署。*下面的内容是基于你已经对Github Action和CI有基本了解的基础上叙述的。*

---

## 思路

&emsp;&emsp;整个工作流可以分为：

1. 配置环境：在CI的机器上需要配置好该项目所需要的运行环境
2. 测试：通过编写单元测试（unit-test）和端对端（e2e test）测试等，对项目中的基本功能进行测试。
3. 构建：将项目构建为可部署的轻量级文件包
4. 部署：将打包后的内容部署到Github Pages上展示。

### 配置环境

&emsp;&emsp;本项目是使用vue-cli搭建的前端项目，因此可以用npm进行开发包的管理。项目的插件配置都放在`package.json`中，因此只需要`npm install`即可。

### 测试

&emsp;&emsp;前端的测试包括一些基本逻辑的测试和接口测试。一般分为单元测试和端对端测试，vue-cli提供了丰富的测试框架。写好测例后只需要运行`npm test`就可以运行测试。

### 构建

&emsp;&emsp;同样地，npm提供了构建的脚本，运行`npm run build `后，项目根目录中就会生成一个`dist/`的文件夹，这个文件夹就是部署静态页面所需要的所有内容。

&emsp;&emsp;为了后续的部署方便，我们将打包的文件夹改名为`docs`，同时修改一些图片的相对路径，以保证静态资源展示不会有误。

&emsp;&emsp;修改`config/index.js`文件如下：

![vue-cli build config](/images/vue-build_config.png)

### 部署

&emsp;&emsp;静态页面的部署既可以是部署到自己的远程服务器上，也可以是借助Github提供的Github Pages上。Github Pages对每个用户提供一个域名，每个仓库都可以配置一个Github Pages，而且还是免费的，对于自己做的小项目非常合适。Github Pages的部署原理非常简单，**就是找到部署目录下的`index.html`文件进行展示**。

&emsp;&emsp;查看仓库下Github Pages的设置，可以打开Settings，向下滑动到Github Pages：

![vue-cli deploy gh-pages settings](/images/vue-deploy-gh-pages.png)

&emsp;&emsp;如图，Github提供四种部署方式：

1. gh-pages branch：使用分支名为`gh-pages`部署
2. master branch：使用主分支部署
3. master branch /docs folder：使用主分支下`docs`目录部署
4. None：不部署到Github Pages

&emsp;&emsp;部署到主分支是不太合理的，因为一般主分支放的都是整个项目的开发代码，开发的代码需要经常修改，而部署的代码不应该人为修改（理论上都是npm打包生成），所以开发的代码和部署的代码最好是分开。同样地，部署到主分支下的`docs/`目录也是不合理的，因为这样子主分支下就多出了一个文件夹。最佳的做法还是将部署的内容放到`gh-pages`分支上，这样对开发人员是透明的。而且也避免了开发过程中不小心修改了部署的代码。

&emsp;&emsp;所以我们需要将构建好的`docs/`文件夹推送到`gh-pages`分支上。我们首先在自己的仓库创建一个`gh-pages`分支（注意名字必须一致）。然后在Github Action中使用插件`peaceiris/actions-gh-pages`（[项目介绍](https://github.com/peaceiris/actions-gh-pages)）上传代码。workflow的部分如下（仅显示构建与部署部分）：

```yaml
- name: npm install, build, and test
      run: |
        npm ci
        npm run build --if-present
        npm test --verbose
- name: Deploy
      uses: peaceiris/actions-gh-pages@v2.5.0
      env:
        ACTIONS_DEPLOY_KEY: ${{ secrets.deploy_key }}
        PUBLISH_BRANCH: gh-pages
        PUBLISH_DIR: docs
```

&emsp;&emsp;其中需要特别注意`ACTIONS_DEPLOY_KEY`的设置。这个变量的设置是用于仓库的权限认证，意思是告诉GitHub运行CI的这台机器有权限把代码提交到对应的分支。我们需要把自己电脑上的**私钥**（一般是位于`~/.ssh/id_rsa`）文件中的内容上传到仓库的secrets中。

![vue-cli deploy secrets settings](/images/vue-deploy-secrets.png)

&emsp;&emsp;至此，整个vue-cli项目的静态页面部署已经完成，我们提交代码后，会自动触发Github Action，根据脚本运行后，我们的页面就已经成功部署到Github Pages上了。我们可以查看`gh-pages`分支验证一下部署的代码是否已经push上去了。

![vue-cli deploy result](/images/vue-deploy-result.png)

---

## 小结

&emsp;&emsp;自动部署完成后，我们就可以专注于功能的开发，而无需顾虑部署的事情，每当我们把代码push到主分支的时候，Github Action会帮我们测试、构建、部署，然后在Github Pages上就能看到新的功能了。这是一个非常实用的技巧，希望这篇博客能够帮助到你，谢谢！