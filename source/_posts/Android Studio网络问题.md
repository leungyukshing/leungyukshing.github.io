---
title: Android Studio网络问题
tags:
  - Android
abbrlink: 4443
date: 2019-02-04 16:06:41
---

## AS中的网络问题

&emsp;&emsp;相信在国内的朋友在使用Android Studio开发时都会遇到无法更新的问题，在下载SDK、各种插件的时候经常报错。原因是这些插件的资源网址都被墙了。其中一种解决办法就是自己下载相关的问题，然后再手动放到对应的目录。但是这种做法非常麻烦，我们还是想利用AS本身提供的管理器来实现。这里就来教大家如何配置。

<!-- more -->

## 步骤

1. 在工具栏打开SDK Manager，危险时可下载更新项，如图：

   ![SDK Manager Platform](/images/SDK Manager Platform.png)

2. 点击SDK Update Sites，显示如下：

   ![SDK Manager Update](/images/SDK Manager Update.png)

3. 以上两步就说明了你的AS无法联网。

4. 登录[站长工具](http://ping.chinaz.com)，输入`dl.google.com`，找一个TTL比较低的IP地址。如图：

   ![Android IP](/images/Android IP.png)

5. 进入本机的hosts文件（一般地址为C:\Windows\System32\drivers\etc\hosts），在里面添加  ：

   ```
          IP地址           主机名
   如 203.208.41.33    dl.google.com
   ```

6. 重新打开SDK Manager就可以下载API了。

## 小结

&emsp;&emsp;这个IP地址可能时不时需要更改，每次就到站长工具里查一个时延比较小的IP就可以了，也算是比较简单的一种解决方法。希望这篇博客能够帮助到你，谢谢！