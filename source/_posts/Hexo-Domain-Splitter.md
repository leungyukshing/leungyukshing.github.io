---
title: Hexo博客优化之域名分流
abbrlink: 14822
date: 2020-04-16 20:13:40
tags:
---

## Intro

&emsp;&emsp;博客的加载速度是非常影响阅读体验的，而很多人都是把博客部署在Github Pages上，这对于国内的访问并不是很友好，甚至有时候会不能访问。为了解决这个问题，我做了域名分流，目的就是提高加载速度，优化体验。

<!-- more -->

## 域名分流的思路

&emsp;&emsp;在之前的博客中我有分享过博客的自动部署（[传送门](http://leungyukshing.cn/archives/Hexo-Travis.html)），其中就有一点是同步代码到coding.net的镜像仓库。当时主要的考虑是备份，但是另一个优点就是，我们同样可以借助coding.net提供的静态页面去部署。这样，我们相当于有了两个部署的网页，一个是Github Pages，另一个是coding.net提供的。

&emsp;&emsp;域名分流就是基于以上假设的。Github Pages可以主要服务于国外的访问，而coding.net可以主要服务于国内的访问，这样网页的加载速度都会提升不少。
![Domain Split](/images/hexo-optimization-splitter4.png)

## 步骤

首先这里假设：

+ 你的个人博客应已经配置了个性化域名，如果没有，请参考[传送门](http://leungyukshing.cn/archives/%E4%B8%BAHexo%E5%8D%9A%E5%AE%A2%E9%85%8D%E7%BD%AE%E4%B8%AA%E6%80%A7%E5%8C%96%E5%9F%9F%E5%90%8D.html)。
+ 你的博客已经使用coding.net作为镜像仓库。

1. 这里我用的是腾讯云，打开域名解析中心。如果使用默认配置，CNAME的解析应该都是默认的。

2. 打开coding.net，添加域名解析并获取coding.net网页地址。

   ![Coding Net1](/images/hexo-optimization-splitter2.png)

   ![Coding Net2](/images/hexo-optimization-splitter3.png)

3. 这个时候，我们只需要把coding.net网页的地址添加到解析中，并设置github的解析为国外，coding.net的解析为国内，这样就实现了分流。

   ![Domain Parse](/images/hexo-optimization-splitter0.png)

## 效果测试

因为博客一直部署在Github Pages，所以优化效果的测试只针对国内的IP访问。

|        | DOMContentLoaded | Load  |
| ------ | ---------------- | ----- |
| 优化前 | 7.3s             | 11.8s |
| 优化后 | 258ms            | 582ms |

![Optimization Result](/images/hexo-optimization-splitter1.jpeg)

## 小结

&emsp;&emsp;可以看到，使用域名分流的优化效果还是非常明显的，尤其是对于国内的访客。希望这篇博客能够帮助到你，欢迎转发、评论！
