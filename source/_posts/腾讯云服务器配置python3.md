---
title: 腾讯云服务器配置Python3环境
date: 2020-01-10 00:03:18
---

## Introduction

&emsp;&emsp;2020年python核心团队宣布将停止对Python2的维护了，那么在这篇博客我就和大家分享一下如何把自己的云端服务器的python环境升级到Python3.

<!-- more -->

+ 服务器环境：Centos7.3
+ 目标Python版本：Python3.6.5

## Steps

1. 首先检查是否有gcc的编译环境，因为后面python的安装需要用到，如果gcc命令没有成功，则安装gcc。

   ```bash
   yum install gcc
   ```

2. 从python官网下载python3的文件，有人觉得服务器中下载太慢，可以选择本地下载好后scp到远程服务器。这里我是直接在服务器中下载的，时间大约为10分钟。

   ```bash
   curl -O https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
   ```

3. 下载好文件后，解压。

   ```bash
   tar -zxvf Python-3.6.5.tgz
   ```

4. 此时，可以看到目录下多出来`Python-3.6.5.tgz`的文件夹。进入文件夹。

5. 创建python3目录，用于放置安装的文件。

   ```bash
   mkdir /usr/local/python3
   ```

6. 执行配置命令和安装程序

   ```bash
   ./configure --prefix=/usr/local/python3
   make
   make install
   ```

   这有可能报错**zipimport.ZipImportError: can't decompress data; zlib not available**，解决办法为：

   ```bash
   cd Modules/zlib
   ./configure
   make install
   ```

   再重新执行`make install`

7. 添加环境变量

   ```bash
   vi /etc/profile
   ```

   在文件夹添加一行：

   ```
   PATH=/usr/local/python3/bin:$PATH
   ```

   然后保存退出，让配置生效

   ```bash
   source /etc/profile
   ```



至此，我们可以输入`python3`，就能看到python3.6.5已经成功安装了。此时系统中有两个版本的python。

## python2和python3共存

&emsp;&emsp;我们想在系统中同时保留两个版本的python，而平时用的比较多的是python3，每次输入命令不想输入`python3`，而是希望直接输入`python`就进入python3的版本。

&emsp;&emsp;这里我采用的是别名的方式去解决这个问题。首先通过`which`命令找到python2和python3的位置。

```bash
which python2
which python3
```

![use which to find python path](/images/which-python.png)

&emsp;&emsp;然后编辑`~/.bashrc`文件添加一下内容：

```
alias python2='/usr/bin/python2'
alias python3='/usr/local/python3/bin/python3'
alias python=python3
```

&emsp;&emsp;然后保存退出，更新配置：

```bash
source ~/.bashrc
```

&emsp;&emsp;再次输入`python --version`，显示的就是python3.6.5版本了。如果想用python2，直接输入`python2`即可。



&emsp;&emsp;本次分享到这里结束了，希望本文能对你有帮助。欢迎评论、转发，谢谢您的支持！