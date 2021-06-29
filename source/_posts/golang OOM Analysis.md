---
title: golang内存暴涨分析
tags:
  - null
mathjax: true
abbrlink: 49884
date: 2020-09-13 00:13:11
---

## Description

&emsp;&emsp;自从升级Go版本到1.12以上之后，服务整天就会有OOM的情况。最近发现在高峰流量过后OOM的情况仍然严重，观察系统内存占用曲线发现，内存使用率曲线呈现一种非常不正常的状态——隔一段时间上升一大段，然后一直没有下降。

&emsp;&emsp;以上就是问题的表象描述，一般会优先考虑goroutine的内存泄漏等问题，下面就来说一下我的排查过程。

<!-- more -->

------

## 排查过程

&emsp;&emsp;首先使用pprof看一下goroutine数量，服务占用的内存等。再对比系统内存占用曲线，发现系统内存占用增多的时候，服务占用的内容仍然保持不变，到这里基本就可以排除掉代码层面上的内存泄漏问题了。

&emsp;&emsp;然后我就把关注点放在了系统层面上的内存使用情况，使用`top`命令发现程序占用的内存非常高，这个很不正常的，所以我就开始怀疑是go的GC机制。这种情况下就没必要自己继续往下查了，上网google一下，果然发现到了问题。

&emsp;&emsp;看到了Go1.12的Release Note对runtime有这么一个更新：

> Go 1.12 significantly improves the performance of sweeping when a large fraction of the heap remains live. This reduces allocation latency immediately following a garbage collection.
>
> The Go runtime now releases memory back to the operating system more aggressively, particularly in response to large allocations that can't reuse existing heap space.
>
> The Go runtime's timer and deadline code is faster and scales better with higher numbers of CPUs. In particular, this improves the performance of manipulating network connection deadlines.
>
> On Linux, the runtime now uses `MADV_FREE` to release unused memory. This is more efficient but may result in higher reported RSS. The kernel will reclaim the unused data when it is needed. To revert to the Go 1.11 behavior (`MADV_DONTNEED`), set the environment variable `GODEBUG=madvdontneed=1`.

&emsp;&emsp;稍微解释一下就是，为了减少频繁的内存申请和释放所带来的latency，Go1.12采用了一种更加aggressive的方法，对于申请到的大块内存，使用完之后并不会真正释放给系统，而是会打上标记，等到系统真的不够内存了，才会去真正释放掉这些空间。所以对于同一个线程来说，如果之后还需要用到内存，它就能直接使用这一块空间，而不需要再重新分配内存、数据清零。但这个缺点就是这些空间没有真正释放之前，它会被记入到进程的RSS（Resident Set Size，一个进程在RAM中实际持有的内存大小）中，因此最终导致的现象就是系统OOM。

------

## Solution

&emsp;&emsp;实际上这个问题是Go语言优化GC做出的一个trade-off，并不是说新的这个带来了问题就不好，而是要看业务选择。这里暂且不讨论什么业务应该用旧的方法，什么业务应该用新的方法，这里只给出fallback回旧方法的策略。

&emsp;&emsp;按照Release Note，只需要设置环境变量`GODEBUG=madvdontneed=1`即可。

------

## Reference

1. [golang issue](https://github.com/golang/go/issues/23687)
2. [madvise详解](https://blog.csdn.net/wenjieky/article/details/8865272)
3. [madvise--Linux manual page](https://www.man7.org/linux/man-pages/man2/madvise.2.html)
4. [golang 1.12 release note -- runtime](
