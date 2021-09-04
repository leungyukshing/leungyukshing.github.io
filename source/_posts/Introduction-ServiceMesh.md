---
title: 浅谈Service Mesh
mathjax: true
abbrlink: 56402
date: 2021-06-05 13:42:17
tags:
---

# Introduction

&emsp;&emsp;微服务兴起后，最近有一个词非常热，叫Service Mesh。很多大厂都极力在推广，这个东西到底是什么呢？在这篇博客我将和大家分享一下有关Service Mesh的知识。

------

# What is Service Mesh

&emsp;&emsp;什么是Service Mesh？中文叫服务网格，它是处理服务见通信的基础设施层。它负责构成现代云原生应用程序的复杂服务拓扑来可靠地交付请求。在实践中，Service Mesh通常以轻量级网络代理阵列的形式实现，这些代理与应用程序代码部署在一起，对应用程序来说不需要感知到代理的存在。

------

# Why Service Mesh

&emsp;&emsp;在了解Service Mesh的工作原理前，首先要知道它诞生的原因，它是解决了软件工程实践中的什么问题。在微服务架构流行并普及后，许多业务都遇到了同一个问题：单个微服务的复杂度大幅度降低，但是系统的整体复杂度并没有下降，反而增加了服务之间连接、管理、监控的复杂度。微服务之间都通过RPC的方式获取数据，每个微服务除了包含本身的业务逻辑外，还需要处理RPC调用的通讯相关问题，包括限流、权限等。这使得开发者无法把精力都集中到业务逻辑的开发中，当业务场景变得复杂后，这对开发者来说是一个更加大的挑战。

&emsp;&emsp;此时，一种通用的框架应运而生，它的基本思路是，把每个微服务中的通讯部分抽流出来统一交给框架处理，而开发者只需要专注于业务开。这样做的好处是，一是开发者可以专注在业务上，而不需要开发过程中过多地关注微服务之间的通讯问题；二是以通用框架的方式去管理微服务可以更加方便运维，包括流量控制、权限控制等。

&emsp;&emsp;这里我用一个简单的例子去说明Service Mesh的作用。在Service Mesh之前，如果我要开发一个微服务A，我需要考虑它的业务逻辑，同时，它对外暴露的接口我也需要做频控、权限控制；如果我再开发一个微服务B，我需要做同样的事情。这里比较理想的做法可能是把频控、权限控制相关的代码都封装成一个通用库，每个微服务都去使用。但其实这种方法也有缺陷，当需要升级时，几乎每个微服务都需要升级一遍。更大的问题是，如果我想禁止掉A对B服务调用某个接口，会很不方便，甚至需要改动的代码来完成。

&emsp;&emsp;当有了Service Mesh后，这一切变得不同。首先无论是开发A还是B服务，我不需要关系通讯相关的问题，我只需要在Service Mesh提供的平台上配置。同时，当我需要禁止掉某个接口时，直接在运维平台上操作即可，而不需要涉及到代码的改动，这个生效时间是非常短的。

------

# Detail of Service Mesh

&emsp;&emsp;这一部分来看看Service Mesh的一些细节及原理。首先来看一张概要图：

![Service Mesh Architecture](https://upload-images.jianshu.io/upload_images/8276716-3c315f1af278b6d7)

&emsp;&emsp;上图中，绿色部分指的是微服务本身，承载着业务逻辑；蓝色部分指的是随着服务部署的服务网格，是作为sidecar运行在同一个容器中；蓝色的线条表示微服务之间的通讯。整个图片看上去蓝色的方块和线条编织起了一个通讯的网络框架，这就是服务网格的意思，所有的服务都在这个网格之上。

&emsp;&emsp;所以服务网格也可以理解为是若干个sidecar服务构成的网络，其中现在业界有很多著名的框架，如Istio等。我们就来看看他们具体是怎么工作的：

1. 首先sidecar会把服务请求路由到一个目标地址，也就是需要把这个请求打到哪个服务上。sidecar会根据请求中的参数判断发往哪个环境（生产环境、预发布环境、测试环境等），是走本地路由还是走公有云环境，这些都是通过读取sidecar的配置去实现的；
2. 当找到目的服务的地址后，就把请求流量发送到相应服务发现端点，在k8s中是service，然后service再转发到后端服务实例；
3. sidecar会根据最近一段时间内请求的延时，选取出响应最快的实例；
4. 然后把这个请求发送到这个实例，记录响应的类型、延时及一些信息；
5. 如果该实例在一定时间内没有响应或者超时，sidecar会把这个请求转发到其他的实例上重试；
6. 如果sidecar检测到某个实例持续返回error，或者延时特别高，就会把这个实例从负载均衡池中移除，再周期性重试，这样就能保证调度的请求都能路由到一些健康的实例中；
7. sidecar以metric和分布式追踪的形式捕获级记录上述的各种行为及参数，并把这些信息发送到集中的metric系统。

------

# Summary

&emsp;&emsp;简单来说Service Mesh为微服务提供了一个监控的框架，能够让业务开发者专注在业务逻辑的开发，而不需要考虑微服务之间的通讯问题。同时也通过框架的形式，提供高可用的通讯框架，以及汇总更多的监控信息用于维护。总的来说Service Mesh会是未来的趋势，许多大厂现在也在极力推Service Mesh。

------

# Reference

1. [什么是Service Mesh](https://jimmysong.io/blog/what-is-a-service-mesh/)
2. [深度剖析Service Mesh服务网格新生代 Istio](https://www.jianshu.com/p/a2cd02118ef1)