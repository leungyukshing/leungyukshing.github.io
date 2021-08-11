---
title: 抢占式优先队列之C++实现
mathjax: true
abbrlink: 58904
date: 2021-08-07 21:48:37
tags:
---

# Introduction

&emsp;&emsp;操作系统的调度中，有一个常用的调度方法叫抢占式调度。在这篇博客我将简单介绍一下抢占式优先队列，并且用C++实现一个简单的版本。

<!-- more -->

# What is Preemptive Scheduling

&emsp;&emsp;抢占式调度出现在操作系统调度的场景中，当两个或多个任务争夺单个CPU计算资源时，就会存在谁先谁后的问题。在抢占式的模式下，优先级较高的任务能够首先被处理。

# Implementation

&emsp;&emsp;这里我将用一个简单的例子去实现一个抢占式调度。

## 问题描述

一个单核CPU按优先级调度进程，进程的优先级是它们的编号，编号越小则优先级越高。输入进程数及每个进程的编号、到达时间、所需的运行时间，求CPU执行进程的顺序。

## 输入

第一行输入进程个数`N`，之后每行输入进程编号、进程到达时间、进程所需运行时间，空格隔开。其中进程的输入顺序是按照到达顺序排序的。

样例：

```
4
9 0 10
7 5 7
2 10 6
4 20 3
```

## 输出

按照进程在CPU的执行顺序，打印进程编号。

样例：

```
9 7 2 7 9 4 9
```

## 分析

&emsp;&emsp;因为要根据优先级进行排序，所以很自然我们会使用优先队列。但这道题目的关键不是优先的问题，而是抢占。

&emsp;&emsp;我们每次都从队头中拿出一个任务，因为使用优先队列，所以这个任务的优先级肯定是最高的。然后我们要看看能处理多久。此时要引入一个变量`t`，表示当前时间减去上一个进程的到达时间，这个表示的是这个时间间隔有多长。然后就是处理抢占任务：

1. 如果`t`大于当前任务所需要的处理时间，则说明当前任务肯定能在当前这一刻前处理完，因此先用`t`减去当前任务的处理时间，然后把当前任务从队列中移除，然后继续从队列中拿下一个任务处理，不断循环这个步骤，直至队列为空或者`t < 0`（说明时间不够了）；
2. 如果`t`小于当前任务所需要的处理时间，则说明当前任务在当前这刻肯定处理不完，因此`t`可以直接置为0，同时还要计算出这个任务还剩多少没处理。计算出来之后，我们需要用这个值去更新这个任务所需要的处理时间（因为变短了），这里特别注意，不能直接修改队列中的值，而是需要把旧的任务先pop出来，然后再push回去。

&emsp;&emsp;最后，如果在最后一个任务到达后，还有任务没有处理完，那么直接按照优先队列里面的顺序逐个处理即可，因为此时已经不存在抢占的问题了。

## 代码

```c++
#include<iostream>
#include<queue>
using namespace std;

// x is proirity
// y is time cost
struct node
{
  int x, y;
  bool operator<(const node &a) const {
    return x > a.x; // ascending
  }
} k;

priority_queue<node> q;
int main() {
  int N;
  cin >> N;
  int i;
  int t = 0;
  for (i = 0; i < N; i++) {
    int x, z, y;
    cin >> x >> z >> y;
    t = z - t;
    if (i != 0) {
      // handle all previous process
      // or there is no more time
      while (t > 0 && !q.empty()) {
        k = q.top();
        if (k.y > t) {
          // if there is no more time, the task with the highest priority will use those time
          k.y -= t;
          cout << k.x << " ";
          q.pop();
          q.push(k);
          t = 0;
        } else {
          // if there is more time, the first task done and continue handling next task
          t -= k.y;
          k.y = 0;
          cout << k.x << " ";
          q.pop();
        }
      }
    }
    t = z;
    k.x = x;
    k.y = y;
    q.push(k);
  }

  while (!q.empty()) {
    k = q.top();
    cout << k.x << " ";
    q.pop();
  }
  return 0;
}
```

# Reference

1. [IBM-Preemptive scheduling](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=scheduling-about-preemptive)
2. [抢占式调度算法](https://blog.csdn.net/adfguochen/article/details/92593386)