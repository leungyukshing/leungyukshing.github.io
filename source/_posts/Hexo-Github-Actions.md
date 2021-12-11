---
title: Github Action配置hexo博客CICD
mathjax: true
date: 2021-12-11 01:52:18
tags:

---

# Introduction

&emsp;&emsp;好久不见。上一篇博客已经是9月份了，写到一半就进入了非常忙碌的模式，没想到一忙就忙到了现在。加上之前的CI/CD流水线需要从Travis迁移，手动发布的模式实在太麻烦，所以就等到现在才恢复更新。

&emsp;&emsp;人类之所以聪明是因为会利用工具，工具使人懒惰。是的，没有了CI/CD流水线，以前手动发布博客的模式实在太过繁琐，写完博客还要敲几行命令等几分钟才能发布完成，实在令人难受。今天就来和大家分享一下使用Github Actions部署CI/CD流水线，实现hexo博客的自动发布。

<!-- more -->

# Background

&emsp;&emsp;还是简单提一下background，之前我是一直采用Travis的CI/CD流水线进行博客的自动发布，配置好之后，我发布博客的步骤实际上就是：

1. 写好文章；
2. 直接push到hexo分支（或者直接上github的网页中upload file，这个功能非常方便，有时外出带了其他笔记本的话，就可以直接利用网页上传文件）

&emsp;&emsp;然而享受了Travis将近两年的免费服务后，Travis宣布把免费的功能取消，如果需要继续使用就要付费。那我肯定果断（也没有很果断，等了半年）把流水线迁走。下一站就是Github Actions，自从被微软收购后，Github推出的一些功能其实口碑都不错，Github Actions就是其中一个，它发展的很快，相关的插件也越来越多，总之满足一个自动发布博客的需求肯定是绰绰有余的。关键是发布的速度还非常快，比起Travis快了将近一倍。（吹这么多希望微软给我发offer）

# Steps

&emsp;&emsp;言归正传，我们来开始配置流水线。

1. 首先需要确认你已经给你的Github仓库配置了Deploy Key（如果之前已经有使用Travis的话，这一步可以忽略）。如果没有配置，打开仓库的Settings，找到Deploy keys，把本地ssh的**公钥（pub后缀）**的内容加进去；

2. 配置环境变量。因为我们需要在运行Github Actions的机器上推送代码，所以我们要把本地的私钥通过环境变量的方式传过去，这样该机器才有权限。同样在仓库的Settings，找到Secrets，把本地ssh的私钥的内容加进去，命名为`GITHUB_KEY`（命名可以随意，但是后面要用到）；

3. 在仓库的根目录创建文件夹`.github/workflows`，在里面创建一个`ci.yml`文件。有关什么是Github Actions和这些文件的位置及命名要求，可以参考[What is Github Actions](https://resources.github.com/devops/tools/automation/actions?utm_source=google&utm_medium=ppc&utm_campaign=2022q2-adv-GLOBAL-Google_Search-Actions_Test&scid=7013o000002CcuMAAS&gclid=CjwKCAiAksyNBhAPEiwAlDBeLJOkF6ejVpaCGJBKz63e3K1iJXBixapzs59zyNGZ79pUKzIQFsIGWhoCzwIQAvD_BwE)，这里focus在配置上；

4. 然后编写`ci.yml`文件，这里我把我自己的share出来作为参考：

   ```yaml
   name: Hexo CI/CD
   
   # trigger when push event happens on hexo branch
   on:
     push:
       branches:
         - hexo
   
   env:
     GIT_USER: leungyukshing
     GIT_EMAIL: jacky14.liang@gmail.com
   
   jobs: # workflow
     build-and-deploy:
       runs-on: ubuntu-latest # run in virtual machine environment
   
       steps:
         - name: Checkout # step 1
           uses: actions/checkout@v2 # format: username/repoName
   
         - name: Use Node.js@14.x
           uses: actions/setup-node@v1 # install nodejs
           with:
             node-version: 14.x
   
         - name: Install depndencies
           run: npm install
   
         - name: Set up environment
           env: # set env
             GITHUB_TOKEN: ${{ secrets.ACCESS_TOKEN }} # token
           run: |
             sudo timedatectl set-timezone "America/New_York"
             mkdir -p ~/.ssh
             echo "$GITHUB_TOKEN" > ~/.ssh/id_rsa
             chmod 600 ~/.ssh/id_rsa
             ssh-keyscan github.com >> ~/.ssh/known_hosts
             ssh-keyscan e.coding.net >> ~/.ssh/known_hosts
             git config --global user.name $GIT_USER
             git config --global user.email $GIT_EMAIL
   
         - name: Deploy # step 3 (deploy github and coding)
           run: npx hexo deploy -g
   ```

5. 这里我监听的是`hexo`分支，这个是我博客源码的分支。还需要注意修改里面的`GIT_USER`和`GIT_EMAIL`。然后还有一点就是，因为我使用了coding.net作为博客的镜像仓库，所以这里我在38行需要把它的域名加到`known_hosts`文件，如果不需要做镜像备份，则可以删掉这一行。

6. 然后把这个文件推上仓库就可以了。然后可以在仓库中跳转到Actions，查看流水线工作情况。

   ![Github Actions](/images/github_actions.png)

   可以看到这里的耗时大概就是1分半钟到2分钟，之前在Travis上的平均耗时大概是3-4分钟，确实是快了不少！

# Summary

&emsp;&emsp;部署完之后发布博客又回到了懒人模式，最近开始放假也会更加频繁的更新，希望这篇博客能够帮助到你，谢谢！

# Reference

1. [利用Github Actions自动部署Hexo博客](https://sanonz.github.io/2020/deploy-a-hexo-blog-from-github-actions/)
2. [Github + Coding实现博客快速自动化部署](https://www.cnblogs.com/sssgoeasy/p/15264517.html)
3. [使用Github Actions自动部署Hexo博客](https://printempw.github.io/use-github-actions-to-deploy-hexo-blog/)