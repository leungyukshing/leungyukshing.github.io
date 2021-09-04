---
title: 什么是存储与计算分离
mathjax: true
abbrlink: 49793
date: 2021-08-27 17:12:48
tags:
---

# Introduction

&emsp;&emsp;最近在看很多新推出的数据库和消息中间件的文档时，都会发现它们主推一个点，叫**存储与计算分离**，那到底什么时候存储与计算分离？为什么现在趋势都是这样，有什么好处？在这篇博客我将和大家简单分享。

<!-- more -->

# What is Storage and Compute?

&emsp;&emsp;我们先来看什么是存储，什么是计算。实际上，我们没有办法完全将两者区分开来，比如说计算机中主要负责计算的是CPU，但是CPU中间也包含着寄存器、高速缓存等存储设备。这里我们只能大致上划分两者，存储指的是数据存储，是长期的；而计算指的是完成逻辑运算或者业务逻辑的部分。

## Storage

&emsp;&emsp;存储是指数据的长期存储，以数据库为例，我们需要落盘数据，那么就需要一定的存储资源，可能是hdd，也可能是ssd，那么这些资源就是storage；再换一个例子，对于消息队列来说，也需要一定时间的数据落盘（一般是7天，特殊场景可能是1年甚至永久），这些也需要存储资源

## Compute

&emsp;&emsp;计算指的是逻辑运算，以数据库为例，执行一条数据库语句包含许多逻辑操作，包括语句分析、优化、索引、写入等，这些都是运算；对消息队列来说，运算可能包含的是消息的订阅、分配、路由等

# Why need to separate storage and compute?

&emsp;&emsp;我们知道一台计算机（服务器）一般来说都会具备计算资源和存储资源，既然我们都把计算资源和存储资源都放到一起，为什么又要把他们分开呢？我们要顺着计算机的发展来解释这个问题。

## Before

&emsp;&emsp;数据库刚出现的时候，是为了记录交易，包括银行交易、订单交易。对于交易系统来说，延时是最重要的因素，一般需要在1s内（200ms最佳）才符合要求。因为当时的网速非常的慢，如果要把存储和计算资源分开的话，花在网络上的时间就会非常多（这个耗时是远大于1s的），因此设计数据库的是就需要把计算资源和存储资源尽可能地放在一起。

## Present

&emsp;&emsp;随着数据量的增大，数据库的作用不再仅仅只是存储数据，许多公司机构都利用各种技术去挖掘出数据的信息，用于各种商业用途，最常见的就是商品推荐、广告投放等，因此使用数据库的方式也就改变了。数据挖掘使用的计算资源自然要比日常操作数据库的资源要大得多，而且使用的频率也不一样。

&emsp;&emsp;再者，如今网络的速度已经比当年快多了，不再是整个过程的瓶颈，相反，现在存储的I/O成为了瓶颈，可以说存储的速度跟不上计算的速度，也跟不上网络的速度。因此需要使用一种更加灵活的方式去设计数据库的架构。

# How to separate storage and compute?

&emsp;&emsp;怎么去把存储和计算分离开呢？我们可以把存储节点和计算节点分开部署，比如部署存储节点就采用更大量的硬盘，而配置更低的cpu等，并且这些可以放到离用户很远的地方，比如山里面（电费便宜）；而计算节点则可以部署比较好的cpu资源，放到离用户更近的地方，更加快速响应用用户请求。

# Advantages

&emsp;&emsp;把存储与计算分离开有如下几个优点：

1. 可拓展性：如果我们想要拓展计算节点，在没有分离的情况下，我们向集群中增加一个节点，这个操作是非常繁重的。因为很多数据库都采用partition的方式存放数据，也就是说每个node都只存放一部分的数据。如果需要往其中添加一个节点，那么就需要对所有的数据进行rebalance，然后把数据从某几个节点copy到新增的节点，这个过程通常需要耗费几天；而如果采用了计算和存储分离，我们只需要简单地增加计算节点即可，因为任意一个计算节点都能够通过网络去访问所有的数据。
2. 高可用性：当某个计算节点损坏或者故障时，并不会影响数据的完整性，其他计算节点仍然可以正确地访问到全部的数据，受影响的是该段时间打到这个节点的请求；
3. 廉价：存储与计算分离开对公司来说最大的好处就是省钱省地方，可以把存储节点放到很远的地方，比如山里面（电费便宜），同时还不需要受到机器大小的限制。

# Summary

&emsp;&emsp;存储与计算分离实际上是一种架构思想，其核心就是把系统中职责分明的两个部分分开管理，因为双方要求的资源不一样，带来的好处就是对双方来说，对方的细节都是透明的，任意一方扩容、故障，都不会影响另一方的使用。因此这种思想逐渐成为了数据库设计和消息中间件设计的主流思想。

# Reference

1. [Separating compute and storage](https://ajstorm.medium.com/separating-compute-and-storage-59def4f27d64)
2. [聊聊计算和存储分离](https://juejin.cn/post/6844904055840440333)