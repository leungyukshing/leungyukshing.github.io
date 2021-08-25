---
title: 浅谈Access Contrl
date: 2021-08-24 12:28:35
tags:
---

# Introduction

&emsp;&emsp;之前在讨论gateway的时候就提到过[https://web.archive.org/web/20160303234840/http://csrc.nist.gov/groups/SNS/rbac/documents/ferraiolo-kuhn-92.pdf](RBAC)，这个到底是一个什么东西呢？实际上RBAC是一种与访问权限控制相关的概念模型，在这篇博客我们将来讨论Access Control相关的细节，以及重点分析下几个比较出名的模型。

<!-- more -->

# What is Access Control

&emsp;&emsp;Access Control，访问权限控制，是一种安全手段，用于指定谁具有查看或使用某种资源的权限，它在计算机领域中是一个非常重要的概念，主要功能是降低风险，保护信息资产。注意，这里我们讨论的都是计算机领域的AC。

&emsp;&emsp;一般来说AC分为physical和logical，physical一般指的是对一些实体的访问，比如对机房、数据中心这些实体的访问；而logical一般指的是通过计算机网络去访问到其中的文件和数据，我们主要focus在后者。

&emsp;&emsp;AC主要有三个model，分别是：

+ Mandatory Access Control (MAC)
+ Discretionary Access Control (DAC)
+ Role Based Access Control (RBAC)

# Details for Access Control

## MAC (Mandatory Access Control)

&emsp;&emsp;MAC是一种**最严格**的AC模型，因为它最初被设计出来是为了给政府部门使用。MAC模型采用的是**层级关系**去管理访问权限，例如level0最高级，level10最低级，那么拥有level0权限的人就能去看所有的资料（这里只是不太严谨地打个比分，只考虑到了classification，没有考虑category，后面会详细补充）。然后我们来看看MAC是怎么工作的？

&emsp;&emsp;首先在MAC模型下，所有的资源都需要被管理员标记（只能被管理员标记，普通用户无法执行标记），标记包括两个信息：classification和category。classification指的是保密分类，比如可以是top secret, confidential, unclassified等等；而category指的是部门、项目、组等，例如行政部门、财务部门等等。同样，每个user也需要有相应的classification和category。

&emsp;&emsp;当用户去访问某个资源时，系统会检查两者个classification和category是否匹配，**必须两者都匹配才允许访问**。可能有人会疑惑，classification高的不就可以访问了吗？试想一下，行政部门的boss和财务部门的boss的classification可能都是top secret，但是他们之间应该是不允许相互查看对方部门的信息的，所以这里要加上category。也就是说，行政部门的boss只能查看属于自己部门的top secret信息。

&emsp;&emsp;这样看来MAC确实非常安全，但是它有两个致命的问题：

1. 可维护性差：所以的资源和用户都需要预先标记，而且标记工作都只能是管理员人工执行；
2. 可拓展性差：只要有新的资源或用户进入系统，则需要管理员手动添加标记，同时如果需要更新配置，比如多加一个classification，或者把一个category拆分成两个小的category，那么就会变得非常麻烦；
3. 用户不友好：用户没有办法把权限交给别人，就算是用户自己的数据，也需要通过管理员给别人授权

## DAC (Discretionary Access Control)

&emsp;&emsp;MAC的几个缺点的根本原因其实都是因为把权限管理的任务都交给了管理员，所以显得不灵活。DAC就截然不同，它允许每个用户对自己的数据进行管理，这样就方便多了。因此这个模型也是目前主流操作系统使用的AC模型。

&emsp;&emsp;我们来看看DAC的工作原理，MAC采用的是给资源标记，而DAC采用的是一个ACL（Access Control List），每个资源都有一个ACL，表明什么人（users、groups）有什么权限去访问它。比如，用户A有一个资源X，它的ACL列表包含：

1. 用户A有full control；
2. 用户B有read access；
3. 用户C有read-write access；
4. Group D有read access；

&emsp;&emsp;因此在DAC模型中，一般来说用户只能对自己own的资源设置ACL。

&emsp;&emsp;DAC模型是一个比较灵活的模型，但同时它也增加了数据泄露的风险，所以flexibility和safety永远都是一个trade off。

## Role Based Access Control

&emsp;&emsp;RBAC最重要的地方就是引入了Role（角色）这个概念，我们先看一下没有Role之前有什么问题。假如在一个公司内部使用DAC，当有新员工入职时，HR就要告诉部门的boss，把这个新员工添加到部门资源的ACL中（因为通常来说HR对部门的资源也是没有权限的，所以HR不能加）；同理，有人离职的时候就需要再把这个人从ACL中移除。在这样一来问题就很大了，因为权限和用户捆绑在一起了。

&emsp;&emsp;而角色这个概念就是用来解决这个问题的。因为在绝大多数场景下，一个人的权限是固定的，不会有太多的变化。例如一个软件开发小组中，某个工程师的代码仓库权限一般就是在一个范围中，因此可以把这个结构化抽象出来，形成了角色的概念。

&emsp;&emsp;以一个业务开发组为例，一般会有的角色是leader、工程师、PM、运维等，PM一般来说是不允许访问到代码的，所以他们直接的权限就直接通过角色预先定义好。然后再把用户分配到某一个角色，完成权限的授予。

&emsp;&emsp;在这种模型下，人员流动只需要更改用户和角色的配置，这个是非常简单和快捷的。同时，对于一个人多个角色来说，这种模型也非常灵活。

&emsp;&emsp;RBAC在具备一定灵活性的同时，也兼顾到了安全。因为预先定义好了角色，所以对于用户的权限是有限制的。DAC模型下很有可能因为人为误操作的原因，给某些用户多添加了权限。而RBAC模型下，权限是加到角色中的，这个是很少变动，较少的变动意味着更少的出错几率。

# Summary

&emsp;&emsp;介绍了几种AC模型，但并不是说越新的模型越好，Access Control一直是一个trade off，如果你想要更加方便灵活，那么隐藏的风险就会更高；相反如果你需要更高的安全性，那么就必须牺牲一些便利。对于安全性要求极高的组织如政府部门等，MAC是一种更好的选择，而对于公司来说，因为流动性极高，而且需要有更高的效率，RBAC是更佳选择。

# Reference

1. [An Introduction to Role-Based Access Control](https://spaf.cerias.purdue.edu/classes/CS526/role.html)
2. [RBAC用户、角色、权限、组设计方案](https://zhuanlan.zhihu.com/p/63769951)
3. [MAC vs. DAC](https://www.ekransystem.com/en/blog/mac-vs-dac)
4. [Mandatory, Discretionary, Role and Rule Based Access Control](https://www.techotopia.com/index.php/Mandatory,_Discretionary,_Role_and_Rule_Based_Access_Control)