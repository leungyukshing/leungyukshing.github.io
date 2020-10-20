---
title: 探索python多线程之谜
tags:
mathjax: true
date: 2020-10-20 12:13:03

---

## Python多线程是真的吗

&emsp;&emsp;多线程是我们在编程的时候一定会考虑到的问题，多线程编程能让我们的代码更好地利用计算资源，提高计算速度和效率。Python作为当下最热门的语言，却在多线程编程方面备受质疑。到底python的多线程真的是多线程吗？在这篇博客我将和大家来探讨这个问题。

<!-- more -->

------

## Python的线程原理

&emsp;&emsp;先直接回答问题，python的多线程确实是伪多线程，也就是说没有办法多个CPU核同时跑多个python的线程。要分析这个问题，我们先要弄懂python的多线程是怎么实现的。

&emsp;&emsp;讨论python的多线程一定要先知道GIL（Global Interpreter Lock，全局解释器锁），它实际上是Python解释器中的一个布尔变量，用作锁。每一个python线程在执行的时候，都需要先获取到GIL，所以在任意时刻，最多只能有一个python线程被CPU执行。所以Python的多线程实际上并没有能够高效地利用CPU资源。

![Python GIL](/images/python_gil.gif)

&emsp;&emsp;为什么Python要引入这样一个锁呢？这就牵涉到了python的内存管理，python是使用引用计数来管理内存的，也就是说对于某一块内存空间，python有一个计数器，记录着一共有多少个地方用到它，如果这个计数变为0的时候，就会释放掉。在多线程环境下，就需要防止这个计数器修改的竞争。如果为每个内存空间都添加一个锁来保证计数的一致性，那么一段代码下来将会有非常多的锁，锁的获取和释放都有很大的开销，会导致性能的下降。更糟糕的时候，在多线程的情况下，还有很大的几率造成死锁。

&emsp;&emsp;既然弄这么多个锁效率又低，还会造成死锁，解决的方案就是用一个锁来管理所有的进程了，这就是GIL的由来。至于为什么python选择这么丑陋的解决方法，其实也是有原因的。python作为一门解释性语言，目的首先是方便实用，在设计的时候在效率方面自然考虑较少。同时，python出现的时候操作系统还没有多线程的概念，当时的机器都是单核CPU，所以也没有考虑到多线程的问题。

---

## 小实验

&emsp;&emsp;口说无凭，我们来验证一下看看python的多线程是不是真的对性能没有提升。

&emsp;&emsp;首先使用单个线程进行运算，记录下时间。

```python
import time
start = time.clock()
def CountDown(n):
    while n > 0:
        n -= 1
CountDown(100000)
print("Time used:",(time.clock() - start))
```

运行时间约为：0.006147s

&emsp;&emsp;然后用两个线程，完成一个同样的任务，同样记录时间。

```python
import time
from threading import Thread
start = time.clock()
def CountDown(n):
    while n > 0:
        n -= 1
t1 = Thread(target=CountDown, args=[100000 // 2])
t2 = Thread(target=CountDown, args=[100000 // 2])
t1.start()
t2.start()
t1.join()
t2.join()
print("Time used:",(time.clock() - start))
```

运行时间约为：0.006618

&emsp;&emsp;对比两份代码的运行时间可以得出结论，多线程并没有比单线程更加高效。

------

## Python怎么做到真正的多线程

&emsp;&emsp;上面分析完之后，难道python就真的无缘多线程了吗？难道用python写代码就真的不能充分利用CPU吗？当然不是，多线程做不到，多进程还是可以的。python提供的多进程接口与多线程很相似，使用起来也几乎一样。同时，可以借助分布式框架`mpi4py`高效实现并行计算。

## 其他编程语言表现的如何？

&emsp;&emsp;python提出GIL的根本原因是它使用了引用计数的方式去管理内存，GIL实际上也是一种折中的解决方案，在频繁的锁管理和多线程并行之间找到一个平衡。那么其他语言是怎么解决这个问题的呢？

&emsp;&emsp;这里以Go为例，Go主要使用的是**Mark and Sweep**，采用标记的方法来对内存进行分类，然后定时或者在内存不够用的时候触发回收，这样做的话实际上就是避开了计数管理。

------

## 小结

&emsp;&emsp;通过分析我们知道了Python的多线程实际上是伪多线程，解决的方法是用多进程替代。同时也通过分析背后的原理，认识了Python的内存管理机制和进程管理机制。
