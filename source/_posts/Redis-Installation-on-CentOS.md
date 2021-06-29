---
title: CentOS下搭建Redis环境
abbrlink: 61849
date: 2020-02-01 15:11:07
---

# Redis环境搭建--CentOS

**Platform**： CentOS7.5

&emsp;&emsp;在我之前的博客中，已经介绍过在win10环境下搭建Redis（[传送门](http://leungyukshing.cn/archives/Redis-Installation-on-Windows.html)）。今天在这篇博客我就来和大家分享一下如何在CentOS系统中搭建一个Redis环境。

<!-- more -->

## 步骤

1. 验证系统中有gcc编译器，如果无，首先安装。后面Redis安装需要用到gcc的环境。

2. 下载redis软件压缩包

   ```bash
   wget http://download.redis.io/releases/redis-5.0.3.tar.gz
   ```

3. 解压

   ```bash
   tar -zxvf redis-5.0.3.tar.gz
   ```

4. 进入文件夹，使用make命令编译C++代码

   ```bash
   cd redis-5.0.3
   make
   ```

5. 指定redis安装路径并安装

   ```bash
   make install PREFIX=/usr/local/redis
   ```

6. 然后进入安装好的文件夹的bin，查看内容

   ```bash
   cd /usr/local/redis/bin
   ls
   ```

7. 里面的内容如下，`redis-cli`就是redis的客户端，`redis-server`就是redis的服务端。

   ![redis bin folder](/images/redis-centos.png)

至此，在centOS上安装Redis服务的整个过程已经完成！