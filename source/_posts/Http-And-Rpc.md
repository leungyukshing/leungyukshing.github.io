---
title: HTTP和RPC
abbrlink: 28480
date: 2021-07-14 22:38:08
tags:
---

# Introduction

&emsp;&emsp;现在的软件工程领域中，经常会听到HTTP（Hyper Text Transport Protocol）和RPC（Remote Procedure Call）这两个术语，好像都是和接口、网络请求有关，但是它们究竟是什么，是不是对等的两样东西，它们之间的关系和优缺点是什么，很多朋友都没有弄清楚。所以在这篇博客，我将简单梳理一下HTTP和RPC相关的知识。

<!-- more -->

---

# HTTP和RPC最大的不同

&emsp;&emsp;HTTP的全称是Hyper Text Transport Protocol，中文叫超文本传输协议，是一种应用层协议，在传输层协议TCP之上。

&emsp;&emsp;而RPC的全称则是Remote Procedure Call，中文叫远程过程调用，是一种服务间的网络通讯模式。它本身和网络层没有任何关系，理论上它可以基于任何网络协议，通常来说它会基于TCP协议。

&emsp;&emsp;所以两者之间最本质的区别就是一个是应用层协议，另一个是服务间的通讯方式。

---

# HTTP详解

&emsp;&emsp;HTTP大家听的最多，接触的也比较多，就先来看HTTP到底是一个什么东西。应用层协议实质上就是一个网络世界的通讯规则。为了让大多数人都能够通讯，所以大家就必须遵守一定的规则去传递信息。比如说HTTP中规定了请求头、报头、请求体等格式，当然还有返回码之类的，大家必须按照规定的格式去通信，相互才能收到消息。

&emsp;&emsp;举个例子，我在浏览器中输入`www.google.com`，从我的机器就能够访问到谷歌的服务器；如果我输入`www.baidu.com`，我也能访问到百度的服务器。这是因为我的浏览器、谷歌的服务器、百度的服务器都遵守HTTP协议，所以我发起这个请求后，它们都能够响应。

&emsp;&emsp;与HTTP处于并列关系的比较常用的协议还有SMTP。这是一个邮件协议，因为世界上主流的邮件服务商都用这个协议，所以不同服务商之间的邮件可以相互通信。

---

# RPC详解

&emsp;&emsp;我们先来看看RPC这个概念是怎么诞生的。与远程过程调用相对应的是**本地调用**。什么是本地调用？其实就是你在代码中调用了一个函数（方法）。以前的服务都是把大部分的功能代码都写在一个服务中，所以逻辑模块之间的交互直接在代码中调用即可。但是后面随着微服务的兴起，庞大的单体服务逐渐被拆分成相对独立的微服务，每个微服务负责一定的逻辑模块和数据模型，那么在写代码的时候，就有很多跨服务的通讯。

&emsp;&emsp;例如我们在做一个外卖的系统，微服务A负责的是餐厅的数据模型，微服务B负责的是菜品的数据模型，这两个是相对独立的模块。当用户查找某样菜品时，就需要这两个微服务通信。怎么通讯呢？最简单的就是用HTTP，我们还是遵循统一的规则，这个肯定是可靠的，但问题是有没有这个必要。

&emsp;&emsp;首先微服务A和微服务B都是我写的，它们之间的通讯实质上是**内部通讯**，没有必要都遵守这么复杂的HTTP。HTTP在DNS解析、HTTP握手、数据包处理等方面都需要大量的耗时，而如果我能够定义内部的通讯规范，那么我就不需要遵守HTTP了。这样一来的好处就是我的通讯变得高效，虽然实际上是服务之间的网络请求，但是做到就跟本地代码调用一个方法这么简单，因此就叫RPC。

![rpc structure](/images/rpc.png)

&emsp;&emsp;一个基本的RPC服务的基本架构如上图，包含了一个组件：

+ Client：客户端，是方法的调用者；
+ Client Stub：客户端存根，存放服务端的地址（通过服务发现），把客户端的请求参数打包（序列化）成网络消息，然后通过网络发送给服务方；
+ Server：服务端，是服务的提供者；
+ Server Stub：服务端存根，接受客户端发过来的消息，把消息反序列化成请求参数，然后处理相应的逻辑。

## 流行的RPC框架

### gRPC

&emsp;&emsp;gRPC是google开源的基于HTTP2.0的协议，在传输数据方面，gRPC使用了protobuf协议，这是一个以超高压缩率著称的数据序列化协议；同时基于HTTP2.0也使得gRPC拥有了更加灵活的流控制以及优先级控制。

### thrift

&emsp;&emsp;thrift是facebook开源的一个框架，它用一个代码生成器来对它所定义的IDL文件生成服务代码框架，好处是这个是跨语言的，劣势是开发者需要学习新的语言特性，有一定成本。

# Comparison

&emsp;&emsp;经过上面的简单介绍，我们可以对HTTP和RPC两者做一些对比和总结：

+ 使用场景
  + RPC主要用于内部通讯，尤其是微服务之间，优点是性能好、传输效率高
  + HTTP主要用于对外通信，优点是适用范围广，缺点是效率较低。

&emsp;&emsp;这里再提几点RPC特有的优点：

1. 长链接：保持长链接减少握手次数，减少网络开销；
2. 服务注册及发现机制：RPC框架有统一的注册中心，配合Service Mesh可以做到有效的监控管理。接口的发布、扩展等能做到平滑无感知；
3. 安全：使用RPC不会暴露资源操作，降低数据风险。

---

# Summary

&emsp;&emsp;到这里相信大家对这两个概念有了一定的了解，在之后的开发中也不会混淆这两个概念。其实两者并不存在孰优孰劣的问题，而是要结合使用场景来选择合适的工具。对于大型的、内部的系统，RPC自然是更优的选择；而对于小型的、对外的系统，HTTP则更胜一筹。