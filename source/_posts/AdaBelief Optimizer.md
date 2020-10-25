---
title: 优化器新起之秀——AdaBelief
tags:
mathjax: true
date: 2020-10-22 11:00:02

---

## AdaBelief

&emsp;&emsp;优化器是训练人工神经网路中一个非常重要的组成部分，优化器的好坏直接决定了训练的精度以及可靠性。我们常用的优化器有SGD、Adam等，最近在NeurIPS 2020中，有一篇关于优化器的paper成为了Spotlight paper，提出了一种新的优化器——**AdaBelief **。实验证明它和Adam一样快，和SGD一样可泛化以及足够稳定，听起来就是当前最优的解法。所以在这篇博客，就来看看这个到底是什么东西。

<!-- more -->

Paper link: https://arxiv.org/pdf/2010.07468.pdf

Paper main page: https://juntang-zhuang.github.io/adabelief/

Github repo: https://github.com/juntang-zhuang/Adabelief-Optimizer

------

## 数学原理

&emsp;&emsp;弄懂optimizer很重要的一个方法就是弄懂其背后的数学原理，优化器的本质就是根据loss函数去指引训练的方向。

![Algoritm](https://juntang-zhuang.github.io/adabelief/img/adabelief_algo.png)

&emsp;&emsp;直接看paper给出算法，很明显AdaBelief是在Adam的基础上改进的，不同的地方用蓝色标出来了。 可以看到$m_t$是动量，在Adam的算法中，$v_t$是没有考虑动量的因素的，而在AdaBelief中，更新$s_t$的时候就考虑到了动量。这个实际上是有什么作用呢？

![](https://juntang-zhuang.github.io/adabelief/img/curvature.png)

&emsp;&emsp;众所周知，loss函数的梯度是一个非常重要的指标，它能够告诉我我们函数是不是在往下走，以及往下走的方向。淡水我们常常忽略了曲率，曲率能告诉我们函数往下走的速度。所以在选择步长的时候，要考虑梯度和曲率。

&emsp;&emsp;在3号位置，应该采取的是较大的步长。先看Adam，$v_t$肯定是会比较大的，因为有一个$g_t^2$，$v_t$作为更新的分母，自然使得整个步长变小了；再看看AdaBelief，$g_t$和$m_t$都很大，但是$g_t-m_t$就比较小了，所以计算出来的$s_t$也是比较小的，$s_t$为更新的分母，整个步长就变大了，所以在3号位置AdaBelief选取的是较长的步长，比较符合预期。

------

## 小结

&emsp;&emsp;单纯从数学推导和paper给出的结果看，AdaBelief是一个不错的优化器，在图像、GAN领域都有不错的效果，但是其实际效果还是要等广泛使用后才知道，毕竟优化器这个东西太过玄学了，在训练中总会遇到各种各种的问题。
