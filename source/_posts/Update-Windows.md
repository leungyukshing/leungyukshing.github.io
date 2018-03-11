---
title: Windows10更新中遇到的一些问题及解决方案
abbrlink: 20449
date: 2018-03-11 11:27:47
tags:
---
&emsp;由于课程原因，需要将本机的系统版本进行升级，需要升级到Windows 10 Fall Creators 版本。本身的版本是14393，目标版本是最新的16299。
&emsp;首先我用windows自带的更新服务，但是完成更新并重启后，提示更新失败了。
<!-- more -->

&emsp;然后就自己找了一下解决方案，以下是我的解决方法：
> + 首先下载win10 易升
> + win + X + a 以管理员权限运行命令行
> + 然后命令行中输入 net stop wuauserv 暂停 Windows Update
> + 接着在路径C:\Windows\SoftwareDistribution中，删除DataStore和Download两个文件夹
> + 再打开win10 易升进行升级
> + 最后完成更新后重新在命令行中输入 net start wuauserv 打开Windows Update 服务

&emsp;整个过程时间大约需要4个小时，耗时比较久，电脑需要一直插着电源和网络。希望这篇post能够帮助到你，谢谢！
