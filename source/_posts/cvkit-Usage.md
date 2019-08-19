---
title: pfm文件预览神器——cvkit
abbrlink: 2429
date: 2019-08-11 18:55:38
tags:
---

## Intro

&emsp;&emsp;在做识别任务的时候，我们会遇到一些网络使用的是pfm文件（Portable FLoat Map），无论是window还是linux下系统基本没有自带的软件可以预览pfm文件。如果我们需要预览pfm文件的话，就会比较麻烦。目前网上许多在线转换的网站都是不行的。今天碰巧找到了一个用于预览的工具cvkit，在这里和大家分享。

<!-- more -->

---

## What is pfm?

&emsp;&emsp;pfm文件经常用于视差图的存储。主要有两部分组成：**文件头**和**元数据区**。

### 文件头

&emsp;&emsp;pfm文件头中属性按行存储，以0xA结束行，属性值以字符格式表示。

+ 第一行：文件标记，'pf'表示单通道（即1个float表示元数据），'PF'表示3个通道（即3个float表示元数据）
+ width
+ height
+ scale：scale > 0 表示大端存储，scale < 0 表示小端存储。

### 元数据区

&emsp;&emsp;由4字节浮点数构成。

---

## Usage

&emsp;&emsp;首先到这里下载对应的版本：[传送门](<http://vision.middlebury.edu/stereo/code/>)。下载后解压安装即可使用。可以配置环境变量，将`cvkit/bin`路径添加到系统变量中，这样我们在任何一个地方都可以使用sv命令。

&emsp;&emsp;这里我需要使用这个工具预览一张pfm文件，打开命令行，进入到pmf文件所在的文件夹，输入如下命令：

```bash
$ sv dispnet-pred-0000002.pfm
```

&emsp;&emsp;然后我们就可以看到预览的结果了：

![pfm文件预览](/images/pfm_preview.png)

## Reference

1. [wikipedia](<http://fileformats.archiveteam.org/wiki/PFM_(Portable_Float_Map)>)
2. [pfm文件格式](<https://blog.csdn.net/dragondog/article/details/81433511>)

