---
title: Git添加含有git管理的文件夹
abbrlink: 50880
date: 2019-08-25 21:59:10
tags:
---

## 问题

&emsp;&emsp;最近遇到了一个有关git的问题，当我们从github上通过`git clone`拉取别人的代码到本地时，如果我们想将这个文件夹作为一个子文件夹添加进我们的版本管理中，会发现原有的版本管理无法追踪到新的这个文件夹。

<!-- more -->

报错提示：

```bash
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
       modified: [some files]
```

但是我们的子文件夹一直都无法被track到。

---

## 原因

&emsp;&emsp;原因很简单，就是因为文件夹下有了自己的git。我们可以通过`ls -a`看到子文件夹下有一个`.git`的文件夹。而版本控制无法递归地检查版本，所以就会出现这个问题。一般来说解决方法有两种，一个是submodule，另一个是直接消除掉子文件夹下的git。这里我们直接用第二种方法解决问题。

---

## 操作

```bash
git rm --cached <folder name>
git add <folder name>
```

---

## 总结

&emsp;&emsp;接下来我会研究一下submodule，希望继续和大家分享git的一些技巧和解决方法。谢谢您的支持！