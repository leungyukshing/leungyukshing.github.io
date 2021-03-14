---
title: Github连接从https迁移至ssh
tags:
mathjax: true
date: 2021-03-13 01:16:02

---

# 问题描述

&emsp;&emsp;Github从2021年8月13日开始就禁止使用账号密码的形式进行验证了，所以最近每次git提交代码都会收到邮件通知。之前一直懒没管，想着一直用https没啥问题，就没什么迁移的动力。但半年后就要禁止了，保险起见最近还是抽空完成了迁移，其中遇到了一些坑，写到这里希望能帮到遇到相同问题的人。

<!-- more -->

------

# https迁移到ssh

&emsp;&emsp;github有两种认证手段，一种是https的账号密码认证，每次认证都需要输入密码（当然也可以通过manager去缓存，但是有一定安全风险），另一种则是ssh，基于非对称加密的认证。前者安全性低，且不方便；后者更加方便，安全性也更高，所以切换到ssh也是有好处的。迁移的过程主要有以下几步：

1. 检查远程仓库的链接到底是https还是ssh，这个和当时git clone下来的方式有关系。命令行输入`git remote -v`就能看到相关信息，如果是以`https`开头的说明当前是https模式，如果是`git@github.com`开头的，说明当前是ssh模式。
2. 如果是https模式，需要切换至ssh。输入命令`git remote set-url origin {ssh-url}`，后面的部分填写仓库的ssh地址。比如我的博客地址：`git@github.com:leungyukshing/leungyukshing.github.iio.git`
3. 输入完后，再次输入`git remote -v`，此时如果远程仓库的url已经更改，说明修改成功。

------

# 使用ssh连接github

&emsp;&emsp;在完成上面部分的切换后，当我们去pull或push代码时，与github的认证就会走ssh，但是需要完成整个认证过程，还需要在本机生成密钥。有如下几个步骤：

1. 检查本机是否有ssh的公钥和私钥。

   ```bash
   $ cd ~/.ssh
   $ ls
   ```

   检查是否存在此目录，如果存在，则说明本机已经生成密钥了；

2. 如果本机没有密钥，则生成一份新的。

   ```bash
   $ ssh-keygen -t rsa -C "your_email@example.com"
   ```

   文件会默认生成放在上面的目录中，一般来说会有两个文件`id_rsa`和`id_rsa.pub`。

3. 然后登陆Github的网页，在个人的Settings中找到**SSH and GPG Keys**选项，向里面添加新的SSH Key，title随便写，然后把上面生成的`id_rsa.pub`文件的内容完全复制到输入框中，点击保存；

4. 在网页上保存后，在本机执行命令验证ssh密钥是否校验正常。

   ```bash
   $ ssh -T git@github.com
   ```

   如果校验成功，会提示：Hi username! You’ve successfully authenticated, but GitHub does not provide shell access. 这个时候说明你的ssh已经可以和github连接上并正常使用了。

------

# SSH中文路径问题

&emsp;&emsp;如果你的系统是Linux或者MacOS，那么这一部分的问题相信你不会遇到，只有非常坑爹的Windows才会碰到这种问题。

&emsp;&emsp;如果你使用的是Windows系统而且你的用户名设置了中文，那么在你使用命令行的进行上面一部分操作时，你的密钥会一直无法生成，这是因为路径中包含了中文字符。此时最快捷的解决方法是，使用git自带的bash，在里面创建密钥。创建完之后，后续使用正常的命令行即可。

------

# Reference

1. [使用ssh连接github](https://www.jianshu.com/p/da621e687c39)
2. [使用SSH协议连接Github](https://blog.csdn.net/u012150179/article/details/24790677)
3. [windows中SSH中文路径问题](
