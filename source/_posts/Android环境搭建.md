---
title: Android环境搭建
tags:
  - Android
abbrlink: 44628
date: 2019-02-04 11:41:17
---

## Android开发环境介绍

&emsp;&emsp;安卓应用的开发需要借助于一些IDE，对于安卓应用而言，比较出名的就是Android Studio和Eclipse。这里我将简要地介绍如何使用Android Studio搭建安卓的开发环境。

<!-- more -->

---

## Andriod Studio下载与安装

&emsp;&emsp;首先我们需要下载和安装Andriod Studio这个开发工具，下载地址为[Android官网](https://developer.android.com)或者是[github提供](https://github.com/inferjay/AndroidDevTools#sdk-tools)，前者在国内可能会被墙，后者一般是没有问题。

&emsp;&emsp;进入亡之后选择对应的版本，如图：![AS下载](/images/Android Studio Download.png)

&emsp;&emsp;然后运行安装问及那，定好Andriod Studio和SDK的默认安装位置就可以了，注意**SDK的安装路径不能有空格和中文字符**，一直点`next`，直至安装完成。这其中基本是没有问题的，如果有报错，大多是因为本机Java版本问题或者java环境变量设置错误。

---

## Andriod SDK的下载和安装

&emsp;&emsp;对于项目开发，我们必须用到的就是SDK（Software Development Kit，软件开发工具包）。这里用到了SDK Manager来帮助我们下载、安装和管理本地计算机上的SDK。

&emsp;&emsp;安装SDK有两种方法，一是直接上官网下载对应的SDK包，然后放到相应的位置，再从Android Studio中配置；二是直接在Android Studio中安装。我个人比较推荐的是使用第二种因为AS会帮我们做好版本管理，不需要自己手动下载和配置。当然，后者的问题在于使用AS下载SDK等资源可能会很慢又或者相应的资源地址被墙，需要手动配置代理。

&emsp;&emsp;这里就先介绍第二种方法。进入Android Studio，在上方工具栏中打开`Tools->SDK Manager`，打开Android Studio自带的SDK管理器，选择自己想要的SDK版本进行下载。过于版本问题，要根据开发的需要选择，过低版本的可能会不支持一些功能，过高的版本支持的机型又会很有限。SDK的下载会比较久，因此在下载时需要时不时留意是否正在下载中。

![AS SDK](/images/Android SDK.png)

&emsp;&emsp;特别需要注意的是，为了结果下载资源被墙或者网速很慢的问题，我给出三种解决方法：一是搭梯子，这样对资源的访问就不会有限制；二是修改AS里面的http代理，换成国内比较快的镜像；三是改host。详细请看这里[传送门](http://leungyukshing.cn/archives/Android%20Studio%E7%BD%91%E7%BB%9C%E9%97%AE%E9%A2%98.html)。

---

## 运行项目

&emsp;&emsp;有了Android Studio和相应的SDK，我们基本上就可以运行第一个Android项目了。首先需要制定一个`workplace`，用于放置工程项目，然后需要指定项目的SDK最低版本，截至选择一个基本的应用框架，我们一般选择`Empty Activity`这个最基本的框架，然后就可以成功创建一个新的项目了。

&emsp;&emsp;先来简单地看一下工程目录，如下图：

![AS工程目录](/images/AS Project Structure.png)

工程目录如下：

+ `manifests`下放置的一般是全局配置文件，用于定义这个应用的一些权限和全局属性。
+ `java`下放置的是业务逻辑控制文件，一般是`.java`，用于描述页面上组件与用户交互的逻辑，还有一些连接数据库、网络访问等功能。
+ `res`下放置的是资源文件，`drawable`和`mipmap`放置图片资源文件，`layout`放置页面布局文件，`value`一般放置样式的定义，如颜色、文字等。

这个就是最简单的文件结构，当然在更加复杂的应用中，应用结构会更加复杂。

&emsp;&emsp;接下来我们需要构建项目，使用`build`就是构建整个项目，Android Studio提供了虚拟机给我们看到应用的运行效果。我们进入`AVD Manager`，这个是模拟器管理器（Android Virtual Device），点击左下角可以创建新的模拟器，我们选择Device的类型以及型号，然后选择API版本，跟着提示做下去就可以了。

![AVD Manager](/images/AVD.png)

&emsp;&emsp;回到开发界面，在完成项目构建后，点击右上方的绿色箭头在虚拟机上运行即可看到效果。默认的项目显示的是`Hello World!`字样！

![项目构建](/images/Android Run.png)

---

## 小结

&emsp;&emsp;整个Android开发环境的配置还是花费了不少时间，主要是下载资源的问题。国内的网络环境确实不太友好，但我们还是有很多解决方法的。AS给我们提供了很方便的开发环境，在开发过程中一定要注意SDK、各个插件包、gradle的版本，保证各个部分之间是相适应的。希望这篇博客能够帮助到你，谢谢！