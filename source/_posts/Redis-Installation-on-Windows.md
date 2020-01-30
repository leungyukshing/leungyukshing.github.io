---
title: Windows下搭建Redis环境
date: 2020-01-30 18:42:27

---

# Redis环境搭建

**Platform**： Windows10

&emsp;&emsp;在我之前的博客中，已经介绍过了Redis的一些基本知识（[传送门](http://leungyukshing.cn/archives/Interview-Redis-basic.html)）。那么Redis最常使用的地方就是后端服务器。碰巧小弟最近做的一个服务器需要缓存阿里云服务认证的token（减少调用api获取token的频率），所以这里就需要用到Redis来提高工作效率啦。今天在这篇博客我就来和大家分享一下如何在windows下搭建一个Redis环境。

<!-- more -->

## 步骤

1. windows版本的Redis在官网上没有，我们需要到Github中下载。

   Github地址：https://github.com/MicrosoftArchive/redis/releases

   我们在Releases看到有最新的版本（Pre Release），但是通常我们都会使用稳定版（Latest Release）。

   ![Redis Github](/images/redis0.png)

   点击zip文件直接下载整个包，这个包解压后就可以使用，无需安装。

2. 下载到本地后，解压。然后打开命令行，进入该文件夹，运行命令：

   ```bash
   redis-server
   ```

   然后我们就能看到redis服务成功启动了。

   ![Redis Start](/images/redis1.png)

   通过提示我们能看到，刚刚我们运行是没有指定配置文件的，所以使用的是默认的文件。reids服务占用的端口号是6379，进程号是9192。

3. 然后我们再开一个客户端（保持服务端运行的命令行），验证redis服务。在该目录下打开命令行，运行命令：

   ```bash
   redis-cli
   ```

   然后我们做两个简单的操作进行验证，一个是`set`，一个是`get`。存储一个键值对，然后拿出来。

   ![Redis Test](/images/redis2.png)

   如图，说明服务正常运行。这也是我们在应用中最常使用的两个操作了。

4. 最后我们在服务器运行的终端操作，ctrl+c终止服务。我们能看到输入提示，redis将RDB快照存到了磁盘上。

   ![Redis Stop](/images/redis3.png)

   我们再到文件夹中查看，发现多出了一个`dump/rdb`文件，这个就是redis落盘的文件。当然，这个文件存储的路径和名字，都是可以通过配置文件进行设置。

至此，在win10上安装Redis服务的整个过程已经完成！