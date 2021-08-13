---
title: 浅谈k8s
mathjax: true
abbrlink: 19619
date: 2021-08-08 14:56:55
tags:
---

# Introduction

&emsp;&emsp;之前聊到云原生架构中k8s是一个非常重要的组成部分，在这篇博客我们就来聊一下k8s，它是一个什么东西，为什么它目前被广泛使用呢？

<!-- more -->

# What is Kubernetes?

&emsp;&emsp;Kubernetes是一个可移植的、可扩展的开源平台，用于管理容器化的工作负载和服务，可以促进声明式配置和自动化。一般来说会简称为k8s，因为k和s之间有8个字符（可能就是因为太长了，所以给了个简称）。

# Why is Kubernetes?

&emsp;&emsp;我们从部署的角度来看k8s在这个过程中起到了什么作用。

## 传统部署

&emsp;&emsp;一开始的时候，应用程序都是直接部署在物理机上面，这样就会存在许多程序都部署在同一台物理机上面。存在的问题就是无法明确服务的资源边界，会存在相互抢占资源、相互影响。比如服务A是出bug了，CPU利用率突然增高，然后就影响了部署在同一台机器上的服务B，这种问题是致命的。为了解决这种相互影响的问题，只能把每个服务都单独部署在同一台物理机上面，这样也会有资源浪费和维护成本高的问题。

&emsp;&emsp;同时对于开发运维人员来说，不同服务需要的操作系统环境不一样，可能为了部署不同的服务需要做大量的环境配置。

## 虚拟化部署

&emsp;&emsp;随着虚拟化概念的兴起，服务部署也进入到了新的阶段。虚拟化技术允许在单个CPU上运行多个虚拟机（Virtual Machine）。虽然只有单个CPU，但是这些VM每个都好像完整拥有一整个CPU。同时，虚拟化的另一个好处是应用程序间隔离，一个VM上的信息不能被另一个VM任意访问到。

&emsp;&emsp;但VM的不足之处也很明显，每一台VM都是完整的计算机，所以它比较重，包含的信息太多，调度起来不方便。大家如果试过在自己的电脑上开VM就知道了。

## 容器部署

&emsp;&emsp;容器（Container）可以理解为是轻量级的VM，和VM一样，每个容器都有自己的文件系统、CPU、内存、进程空间等。其优点是与基础架构分离，所以开发者并不需要知道底层的OS是什么，这样的特点使得容器非常容易部署和移植。

&emsp;&emsp;容器部署的一些好处：

1. 快速部署：与VM相比，使用容器部署具有更好的简便性和更高的效率；
2. 持续开发、持续集成：基于容器的服务变更更加快速和可靠，包括细粒度的发布、回滚等‘
3. 环境一致性：对于容器来说，它在开发、测试、生产环境的表现是一样的，这样可以减少由于环境不一致而导致的生产事故；
4. 资源利用：更细粒度的容器能够提高资源利用率，也方便调度。

# Detail for Kubernetes

&emsp;&emsp;这部分来看看k8s提供了什么基本的功能。

1. **服务发现和负载均衡**：k8s使用DNS名称或者IP地址公开容器，如果进入某个容器的流量过大，k8s能够负载均衡把流量分配到其他的服务中，从而使得服务整体稳定；
2. **存储编排**：k8s允许自动挂载自己的存储系统，让存储的生命周期和容器的生命周期有一个连接；
3. **自动部署和回滚**：可以按照配置实现自动部署和自动回滚；
4. **自动完成装箱计算**：装箱（shipping）实际上就是调度，把一个容器放到一个集群的某一个机器上。用户可以指定某个容器需要多少的资源，然后k8s通过计算，把这个容器放到能够满足资源需要的机器上；
5. **自我修复**：在集群中，经常可能出现宿主机的问题或者OS的问题导致容器不可用，k8s能够对这些容器进行恢复；
6. **密钥与配置管理**：k8s能够存储和管理敏感信息，而不需要重建容器实现热部署。

## Infrastructure

&emsp;&emsp;在这部分我们来看看有关k8s的架构，我们先从宏观入手，然后再去了解每一个部分的细节。

### 整体架构

![k8s](/images/k8s.png)

&emsp;&emsp;k8s整体上是一个server-client的架构，master是管理节点，Node是实际工作节点。所有的UI、CLI只会和master进行沟通，然后master再把这些命令下发给响应的节点。

### Master架构

![k8s master](/images/k8s_master.png)

&emsp;&emsp;master部分包括了4个部分：

1. **API Server** ：这个模块用来处理API请求，k8s系统中所有的组件都会和API Server连接沟通，传递消息或命令；
2. **Controller**：控制器主要是对集群状态的一些管理。比如自动对容器进行修复，自动水平扩张等；
3. **Scheduler**：调度器的作用是，把用户提交的容器，依据它对CPU、内存的大小要求，找到合适的Node放置；
4. **etcd**：etcd是一个分布式存储系统，API Server中所需要的所有原信息都被放置在etcd中，它本身的高可用性保证了整个k8s master节点的高可用；

### Node架构

![k8s node](/images/k8s_node.png)

&emsp;&emsp;Node节点是k8s中**真正运行业务负载的**，每个业务负载以Pod的形式进行，每个Pod里面运行多个容器，而去运行这些Pod的组件就是Kubelet。然后我们还看到有其他的部分，Container Runtime是启动Pod时的组件，Storage Plugin和Network Plugin是当Pod需要进行网络或存储操作时需要用的组件。而Kube-proxy是用来构建k8s网络的，k8s也有自己的网络，每个Node使用kube-proxy来组建网络。

## Element

### Pod

&emsp;&emsp;Pod是k8s中的最小调度单位和资源单位，用户通过API生产一个pod，然后让k8s对pod进行调度，最终把这个pod放到某个Node上。而一个pod是一组容器，每个pod里面可能会包含一个或多个容器。

&emsp;&emsp;举个例子，一个pod包含两个容器，容器1指定了资源是1核1G，容器2指定了资源是1核2G。然后还包括其他的资源，比如Volume的存储资源，这个在下面将介绍。

&emsp;&emsp;同时在Pod中也可以定义不同容器的启动命令，pod这个抽象概念为里面的容器提供了共享的运行环境，它们共享一个网络环境，所以这些容器之间可以用localhost来直接访问。

### Volume

&emsp;&emsp;Volume的中文是卷，它是用来管理k8s存储的，用来声明在pod中的容器可以访问文件目录，一个volume可以挂在在一个pod中一个或多个容器下。

&emsp;&emsp;Volume本身而是一个抽象的概念，它既可以是本地的存储，也可以是云存储。

### Deployment

&emsp;&emsp;Deployment是在Pod的上层，它定义一组Pod的副本数量，以及这个Pod的版本。一般大家都会用Deployment这个抽象来做应用真正的管理，所以Pod是组成Deployment最小的单元。

&emsp;&emsp;k8s对Pod的管理也是通过Deployment实现的，它首先通过Controller去维护Deployment中Pod的数目，同时也会自动恢复里面失败的Pod。还有其他的一些发布策略、滚灯升级、回滚等，都是通过Deployment实现。

### Service

&emsp;&emsp;通常在一个Deployment中有多个完全相同的Pod，对于外部应用来说，它只是想访问这些Pod中任意的一个，而不需要指定某一个，所以这里就存在负载均衡的情况。Service就是一个抽象出来的概念，类似于VIP（Virtual IP），是统一提供给外部访问的稳定地址。

### Namespace

&emsp;&emsp;namespace是用来在集群内部的逻辑隔离的，包括鉴权、资源管理。Pod、Deployment、Service都属于一个Namespace，同一个Namespace中的资源需要命名的唯一性，不同的Namespace中的资源可以重名。

# Summary

&emsp;&emsp;上面是对k8s一些基本的介绍，这篇博客重点是介绍了一些基本的架构，至于k8s的实战，就留到后面再说了（太麻烦了哈哈哈）。

# Reference

1. [Kubernetes是什么--官方](https://kubernetes.io/zh/docs/concepts/overview/what-is-kubernetes/)
2. [Kubernets是什么](https://zhuanlan.zhihu.com/p/29232090)
3. [从零开始入门k8s：详解k8s核心概念](https://www.infoq.cn/article/knmavdo3jxs3qpkqtzbw)