---
title: Android运行错误之xml控件问题
tags:
  - Android
abbrlink: 45155
date: 2018-08-21 14:06:06
---
&emsp;&emsp;在Android Studio在虚拟机上运行程序的时候，程序直接终止，报错信息为**caused by NullPointerException: Attempt to invoke virtual method 'boolean java.lang.String.equals(java.lang.Object)' on a null object reference**.
<!-- more -->

&emsp;&emsp;截图如下：
![error](/images/error.png)

&emsp;&emsp;这是控件view的问题，找到对应的xml文件，发现有一个地方的`View`写成了`view`（**注意大小写**）。改回大写后就没有问题了。
