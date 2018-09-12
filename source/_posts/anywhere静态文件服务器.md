---
title: 搭建Anywhere静态文件服务器
abbrlink: 43047
date: 2018-08-26 13:19:55
tags:
---
## 介绍
&emsp;&emsp;在做开发的过程中，访问数据时碰到了浏览器跨域访问的问题，虽然可以通过设置浏览器的安全属性解决，但是整个过程比较麻烦。相比之前，搭建一个本地服务器，直接运行网页会更加方便。npm包管理中给我们提供的[Anywhere](https://www.npmjs.com/package/anywhere)就是一个随启随用的静态文件服务器。
&emsp;&emsp;在官网中它的介绍是这样的：*Runnning static file server anywhere.*即随时随地将一个目录变成一个静态文件服务器的根目录。
<!-- more -->

## 安装
**请确保已经有npm**
安装过程如下：
  + 打开命令行输入：`npm install anywhere -g`（全局安装anywhere）
  + 进入到根目录，输入`anywhere`即可，这时系统默认浏览器会自动打开主页。

## 小结
&emsp;&emsp;使用anywhere可以非常方便地解决涉及到数据访问的浏览器跨域问题，因此在这里推荐给大家，谢谢！
