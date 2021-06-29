---
title: QUIC协议介绍
abbrlink: 56896
date: 2020-05-31 11:43:12
tags:
---

# QUIC协议介绍

&emsp;&emsp;QUIC(**Q**uick **U**dp **I**nternet **C**onnection) 是由Google提出的使用UDP进行多路并发传输的协议，直接对标TCP。这里结合我个人的理解，对其中一些技术要点进行分享。

<!-- more -->

## What is QUIC (general)

&emsp;&emsp;我们先从总体上来看一下QUIC是什么。先看下面这段引用自Chromium Projects的介绍。

>    QUIC is a  new transport which reduces latency compared to that of TCP. On the surface, QUIC is very similiart to TCP+TLS+HTTP/2 implemented on UDP. Because TCP is implemented in operating system kernels, and middlebox firmware, making significant changes to TCP is next to impossilbe. However, since QUIC is build on UDP, it suffers from no such limitations.

&emsp;&emsp;这部分很直接地说出了QUIC出现的必要性——为了解决TCP延时过高的问题。同时因为存在TCP在多年的发展中中间设备僵化、依赖操作系统的实现导致协议僵化这两个问题，使得在TCP协议上进行改造变得非常困难。而且现阶段普遍使用TCP+TLS+HTTP/2的方案存在着诸多问题，如TCP本身带来的队头阻塞问题、握手时间过长问题等，所以Google就提出了QUIC协议，目的就是减少网络延时，在业务逻辑不需要做任何优化的情况下，或者在弱网络环境的情况下，提升网络响应速度，给网页应用更佳的体验。

---

## Details

&emsp;&emsp;前面说到了当前方案（TCP+TLS+HTTP/2）的不足，主要是TCP和TLS带来延时的问题，我们先来看看是什么导致了延时。

1. TCP三次握手和TLS四次握手一共3个RTT。
2. TCP队头阻塞。

### RTT(Round Trip Time)

&emsp;&emsp;RTT指的是**客户端从发送一个请求，到接收到相应之间间隔的时间。**RTT一般包含信号的物理传输时间和数据包在路由过程中解析的时间。RTT会很影响网络应用的体验，所以优化RTT是提升网络应用体验的一大方向。

&emsp;&emsp;TCP的三次握手包含了1个RTT：客户端请求，服务器应答。TLS的连接需要四次握手共2个RTT，第一个RTT用于客户端获取服务器提供的public证书，第二个RTT用于客户端把自己随机生成的key通过公钥加密后传输给服务器，用于后续内容的加密。

&emsp;&emsp;QUIC也是从这三个RTT入手进行优化的，先来看TCP的三次握手带来的1个RTT是否可以优化掉。其实这一次握手最主要的作用就是服务器端对客户端的身份进行确认，这一个RTT没有传输任何有意义的数据，那么一个很直观的优化思路就是：能不能在这一个握手的时候，就带上需要传输的数据呢？实际上TCP是支持这种方式的，客户端发起请求的SYN握手请求的时候就可以带上数据包，但是TCP协议的实现方**不允许**把这个数据包上传到应用层，这主要是为了防止TCP的泛洪攻击。这就是说，如果我们希望在SYN握手请求的时候带上数据包，那么我们将会面对TCP泛洪攻击的风险。TFO（TCP Fast Open）就是为了这种场景而设计的一种方案，它支持在发送SYN请求的时候，同时带上数据包，并确保安全性。

&emsp;&emsp;TFO的设计实质上就是借鉴了Sesstion Ticket的思路，它使用cookie标识客户端的信息，在第一次握手的时候由server生成，保存上次会话的配置信息。所以在**除了第一次TCP连接之外**（第一次TCP连接仍然需要三次握手验证身份），在客户端发起的后续的TCP连接时，都会直接是SYN+Cookie+数据，然后不需要等待服务端ACK就继续发送后续的数据包。当然，如果服务端对Cookie的验证不通过，那么就会回退到三次握手的步骤，上一次传输的数据无效。尽管有cookie的保护，但是如果先拿了cookie，然后在发起攻击，还是有可能在短时间内使得服务瘫痪，所以服务端的端口都会记录一个`PendingFastOpenRequests`的参数，用于记录多少个请求时通过TFO的，如果超过了上限就直接忽略到，避免服务器受到攻击。这样，借助TFO，我们就可以在优化掉这个TCP三次握手的RTT。

&emsp;&emsp;然后我们来看一下如何去优化TLS的两次RTT。TLS的第一个RTT是用于验证身份的，第二个RTT是用于传递加密的key的。第一个RTT的作用和TCP的RTT基本相似，所以可以采用Sesstion Ticket的思路，把校验信息缓存下来，这样就能够优化掉第一个RTT。

&emsp;&emsp;所以这里我们就成功地把3个RTT减少到1个RTT，在极简情况下，QUIC甚至能够优化到0RTT。（参考：[Facebook App对TLS的魔改造: 实现0-RTT](https://blog.csdn.net/hzw05103020/article/details/54974563)）

![TCP+SSL的三个RTT](/images/tcp_tls_handshake.png)

### HOL(Head of Blocking)

&emsp;&emsp;队头阻塞，是所有的管道模型都一定会遇到的问题。它指的是，当有多个**串行请求**执行的时候，如果前面的一个请求不执行完毕，后面的请求将会一直等待。这种情况会造成后面的请求等待时间很长，同时也不能很好地利用网络带宽。

&emsp;&emsp;在HTTP协议中，为了**缓解**这个问题，引入了并行的模式，允许客户端发起多个并行请求。在并行的情况下，即使某一条管道的某一个数据包block了，也只会影响这条管道上后面的数据包，而不会影响其它管道上的，所以这种方案能够有效提升网络带宽的利用率。但需要注意的是，上面我用的时候**缓解**，也就是说并行请求的方案只能是减少HOL带来的不利影响，而不能真正解决这个问题。

&emsp;&emsp;解决block这个问题最关键的地方就是解决数据包传输的顺序问题。如果在传输的过程中，我们对数据包的顺序没有严格的要求，那么这个问题就能够解决了。SPDY提供了一种新的思路——**多路复用**，即所有数据包不分先后地进行传输。也就是所就算前面一个包失败了，后面的也可以进行传输，这样做的代价就是需要在每一个数据包都带上标记，这个标记是给客户端最后进行拼接的。说到这里是不是有一些熟悉？这个和TCP滑动窗口机制的本质是一样的。TCP的滑动窗口机制是为了平衡传输效率和保证数据的可达性，在ACK丢失或出错的情况下，确实能够避免HOL，但是如果是数据包发生了丢失，HOL还是会出现。所以，我们需要有一套机制，**在没有顺序的情况下，保证数据的可达性**。

&emsp;&emsp;操作系统中有一个文件存储的方式叫RAID5，其中它用于校验文件正确性的技术叫前向纠错（Forward Error Correcting）。这里保证数据的可达性也是借用了这种思路，它最直接的作用就在多个数据传输的过程中，如果只有**一个数据包发生丢包或者错包**，能够自动纠正过来。其原理如下：

```C++
// 异或运算
a ^ a = 0
a ^ 0 = a

// 假设
A1 ^ A2 ^ A3 ^ .. ^ An = T
    
// 通过运算使得等式左边只有一个元素
(A1 ^ A1) ^ A2 ^ A3 ^ ... ^ An = T ^ A1
A2 ^ A3 ^ ... ^ An = T ^ A1
A3 ^ ... ^ An = T ^ A1 ^ A2
Ai = T ^ A1 ^ A2 ^ ... Ai-1 ^ Ai+1 ^ ... ^ An
```

&emsp;&emsp;上面的每一个$A_i$都可以看作是一个数据包，$T$看成是所有数据包异或的结果，也被称作为冗余数据，所以最后推导的结果就说明，**任意一个数据包，都可以由其余的数据包加上冗余数据推导出来**。因此在这种设计之中，一个丢包或错包是允许的。

&emsp;&emsp;因此QUIC利用冗余数据，能够在数据包乱序传输的情况下保证数据的可达性（一个丢包可恢复，多个丢包就需要重传）。

---

## Summary

&emsp;&emsp;总的来说这篇博客讨论的是QUIC如何从latency的方向去优化现有的方案，其中借用了Session Ticket和FEC两个技术，这个是我觉得比较有趣的地方。讲解的内容是从我的角度出发理解，如果有任何补充和意见，欢迎直接联系我，感谢支持！

---

## Reference

1. [IETF QUIC Specification](https://tools.ietf.org/html/draft-ietf-quic-transport-27#page-6)
2. [Chromium: QUIC, a multiplexed stream transport over UDP](http://chromium.org/quic)
3. [QUIC: Design Document and Specification Rationale]()
4. [试图取代TCP的QUIC协议到底是什么](https://xiaozhuanlan.com/topic/2083674195)
5. [QUIC协议原理分析](https://zhuanlan.zhihu.com/p/32553477)
6. [IETF TCP Fast Open](https://tools.ietf.org/html/rfc7413)
7. [Introducing Zero Round  Trip Time Resumption (0-RTT)](https://blog.cloudflare.com/introducing-0-rtt/)
8. [Forward Error Correction](https://webcitation.org/65iNkn800?url=http://www.aero.org/publications/crosslink/winter2002/04.html)