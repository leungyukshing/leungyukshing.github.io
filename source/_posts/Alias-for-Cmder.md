---
title: cmder设置alias
tags:
date: 2018-10-19 01:06:52
---

## cmder设置alias

&emsp;&emsp;alias是命令行中一个很强大的功能，设置各种各样的alias命令能够让我们开发过程中得心应手。每个人都会有自己独特的一套alias，在windows环境下，使用cmder设置alias能够提高我们的工作效率。

<!-- more -->

设置步骤如下：

1. 找到alias设置文件，一般在cmder安装目录下的`/config/user-aliases.cmd`。进入文件添加alias命令。

2. 可以参考文件中已经存在的一些命令，这里我提供一个例子，如设置`git status`命令的alias为`gst`，只需要在最后一行添加：

   ```cmd
   gst=git status
   ```

3. 保存文件后，打开命令行输入：

   ```bash
   alias /reload
   ```

   然后会显示`Aliases reloaded`字样，说明alias设置成功。

4. 如果是win10的用户，在新版本可能仍然不能使用。这是我们需要打开`cmd`，在标题处右键打开`propertyies`设置，勾选`Use Lagacy Console`。确认后重启cmder即可。



cmder设置alias的教程就到这里了，这个trick我认为是非常有用，希望能够帮助到大家，谢谢！