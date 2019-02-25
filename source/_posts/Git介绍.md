---
title: Git介绍
tags:
  - Git
abbrlink: 3903
date: 2019-02-09 23:18:16
---

## Git 介绍
&emsp;&emsp;Git是开发人员经常会使用到的一个工具，它到底是什么呢？在这篇博客我将简单地为大家介绍一下Git。
<!-- more -->
&emsp;&emsp;在Wikipedia上，对Git的介绍是这样的：

*Git is a version control system for tracking changes in computer files and coordinating work on those files among multiple people. It is primarily used for source code management in software development, but it can be used to keep track of changes in any set of files. As a distributed revision control system, it is aimed at speed, data integrity, and support for distributed, non-linear workflows.*

&emsp;&emsp;简单来说，Git是一个当前比较流行的**分布式版本控制系统**。它能够帮助我们对代码文件进行版本控制。所谓版本控制，就是对于同一份文件，我们会做多次的修改，版本控制提供给我们类似于历史记录的功能，记录每次的改动（增、删），这使得我们在发生错误时可以进行版本回退，而不需要每次修改后都重新保存一份新的副本。

------

## 日常使用

### 个人代码管理

&emsp;&emsp;Git的最基本使用就是在**本地仓库**和**远程仓库**之间的关联，包括了推操作`push`和拉操作`pull`。本地仓库，是指在本地计算机上的某个文件夹，我们可以指定任意一个文件夹为一个仓库。远程仓库，就是在一些平台上的在线仓库，最出名的平台当然就是GitHub了。将文件从远程仓库拉取到本地，称为`pull`；将文件从本地仓库推送至远程仓库，称为`push`。常用的命令如下：

1. 初始化本地仓库

   ```
   mkdir testDir
   cd testDir
   git init
   ```

   这样，我们首先创建一个名为`testDir`的文件夹，进入该文件夹后，初始化为一个本地仓库，一个隐藏文件夹`.git`会自动生成。

2. 推送代码至远程仓库。我们新增一个文件`README.md`，推送到远程仓库

   ```
   touch README.md

   git add README.md   // 只添加README.md这一个文件
   git add .     // 添加所有文件

   git status // 查看缓冲区信息

   git commit -m "add README" // 发起推送请求，-m后添加说明

   git push origin master // 将代码推送至远程仓库的master分支
   ```

3. 从远程仓库中拉取文件。如果我们和别人合作开发，就需要fork别人的仓库到自己的仓库中，然后再拉取到本地：

   ```
   git clone https://github.com/用户名/仓库名.git
   ```

   如果别人仓库有更新，我们只需要

   ```
   git pull

   或
   git fetch
   git merge
   ```

   这样就可以把主分支的最新内容更新到本地了。详细的操作在后面分析。

4. 在第一次使用git的时候，还需要配置全局的用户信息。

   ```
   git config --global user.name "用户名"
   git config --global user.email "邮箱地址"
   ```

   对于`push`操作，如果觉得每次`push`都需要输入密码很麻烦的话，可以使用ssh的方式来认证。步骤如下：

   1. 生成秘钥

      ```
      ssh-keygen -t rsa -C "邮箱地址"
      ```

      一般使用默认的就可以（直接回车），没有密码。

   2. 完成后会有生成两个文件：`id_rsa`和`id_rsa.pub`，前者是私钥，后者是公钥。一般在`C:\Users\用户\.ssh`文件夹中，然后打开GitHub网页中的`setting`，添加public key，将`id_rsa.pub`中的内容复制进去就可以了。

### 团队协作开发

&emsp;&emsp;Git的另一个很强大的功能就是帮助开发者进行团队协作开发。其基本思路是，一个人是项目的管理者，其他人是项目的协作开发者。每个人都可以对这个项目进行开发，开发后可以将自己的代码提交给管理者，管理者可以根据需要同意或者拒绝合并其他人的代码。这一个功能非常方便而且可靠。下面介绍一些基本步骤与常用操作：

1. 从本地将代码推向远程仓库，与上面一样。注意这个远程仓库是自己的远程仓库。
2. 将自己的提交pull request给管理者。所谓的PR（pull request）是一种通知机制。你修改了他人的代码，将你的修改通知原来的作者，希望他合并你的修改，这就是Pull Request。Pull Request本质上是一种软件的合作方式，是**将涉及不同功能的代码，纳入主干的一种流程**。这个过程中，还可以进行讨论、审核和修改代码。
3. 如果管理员认可你对该项目的提交，将同意你的Request，使用merge将你的提交合并到主分支上。
4. 如果你需要在本地同步主分支上的代码，需要先fetch主分支，然后进行merge，也可以直接pull主分支。pull可以看做是fetch和merge的结合。一般如果有代码冲突的话，在fetch后merge前需要手动解决冲突。

&emsp;&emsp;团队开发过程中建议使用Github的客户端或者SourceTree客户端，对于管理代码等非常方便。

------

## 小结

&emsp;&emsp;Git是我们在日常开发中能带给我们方便的工具，最近Github也提供了免费的私人仓库，大大增强了代码的保密性。在入门的时候大家最后使用命令行来操作，这样能够更好地熟悉Git的原理；而在项目开发过程中，就可以使用Github、SourceTree这些GUI来管理代码。希望这篇博客能够帮助到你，谢谢。
