---
title: 实习面经--计算机网络基础
tags:
  - 面试总结
mathjax: true
abbrlink: 51449
date: 2019-06-30 17:20:45
---

# 面试总结之计算机网络基础

&emsp;&emsp;计算机网络是计算机领域的重要组成部分，无论是前端开发或者是后端开发，多多少少都会涉及计算机网络的一些基础知识，这里就为大家总结一下常见的考点。总结的部分是结合了教科书、其他面经以及个人的面试经历，相信能够帮助到大家。如果有不足的地方，欢迎大家指出！

&emsp;&emsp;本博客长期更新，欢迎大家收藏、订阅！

<!-- more -->

## TCP的4次挥手

&emsp;&emsp;TCP握手是3次，挥手是4次。握手大家都比较熟，面试的时候问挥手比较多。

+ 过程：
  1. Client发出终止连接的请求，FIN置为1
  2. Server收到后，返回一个ACK，表示Server知道Client需要终止连接
  3. 因为Server可能还有没有传输完的数据，所以等到数据都传完后，Server向Client发送一个数据，FIN置为1，表示Server已经发送完数据了，可以中止连接。
  4. Client收到后，发送一个ACK，确认终止连接
+ Details：在上述第四步后，Client会进入`time_wait`状态，这个是常问的点。原因有二：
  + 让Client有更大的机会让丢失的ACK被再次发送给Server
  + 不允许在`time_wait`状态建立连接，经过2MSL后变为`CLOSE`状态，这样上一个连接的残余数据都会被丢掉，不影响后续的连接。

![TCP的4次挥手](/images/tcp_shakehand.png)

---

## TCP如何保证传输的可靠性

1. 应用数据被分割成TCP认为最适合发送的**数据块**。
2. **超时发送**：TCP发出一个段后，它启动一个定时器（**timer**），等待接收端返回ACK确认发到这个报文段。如果不能及时收到一个ACK确认，将重发这个报文段。
3. TCP接收端收到数据后，将发送一个ACK确认。
4. TCP保持它首部和数据的校验和（**checksum**），是端对端的（**end-to-end**）。如果检验出包有错，丢弃报文段，不给出响应，TCP发送端超时后重发。
5. TCP按照seq对收到的数据进行重新排序，将收到的数据以正确的顺序交给应用层。
6. **流量控制**：滑动窗口机制，防止较快主机致使较慢主机的缓冲区溢出。

---

## TCP拥塞控制（congestion control）

+ **目的**：防止过多的数据注入到网络中，这样可以使网络中的路由器或链路不致过载。（注意，**拥塞控制（congestion control）**针对的**整个网络**流量的控制，而**流量控制（flow control）**针对的是**端对端**的控制）

1. **慢启动（slow start）**和**拥塞避免（congestion avoidance）**
   + 发送方为出一个拥塞窗口`cwnd`，窗口大小取决于网络的拥塞程度。`cwnd`从1MSS开始，按1，2，4，8的方式增加（**倍增**）
   + 慢开始门限`ssthresh`
     - `cwnd`$\lt$`ssthresh`，慢启动；
     - `cwnd`$\gt$`ssthresh`，拥塞避免；
     - `cwnd`=`ssthresh`，两者都可
   + **拥塞避免**：`cwnd`线性增加，每次增加1MSS。
2. **快速重传（fast retransmit）**和**快恢复（fast recovery）**
   + 快速重传：接收方每收到一个失序报文就发送出**重复的ACK**，一旦收到3个相同的ACK，则重传该报文段
   + 快恢复：当接收方收到3个相同的ACK时，`ssthresh`减半，重置`cwnd=ssthresh`，然后`cwnd`执行线性增。原因是接收方能够收到ACK说明网络中仍有一定的传输能力，因此不需要重新开始。
3. 例子
   + **TCP Tahoe**：当发生丢包（超时或收到3个ACK）时，`ssthresh=cwnd/2`，`cwnd=1`，执行**慢启动**；
   + **TCP Reno**：当收到3个ACK时，`ssthresh=cwnd/2`，`cwnd=ssthresh`，执行**拥塞避免**；

---

## HTTP和HTTPS

+ HTTPS，称作HTTP over TLS，TLS的前身是SSL。

+ 原理：首先Client和Server双方使用**非对称密钥**进行沟通，将Client端生成的一个key通过非对称加密的形式传递给Server。然后双方使用key进行**对称加密**，从而进行正常的内容传输。

+ 区别：

  + HTTP是明文传输，HTTPS则是具有安全性的SSL加密传输协议
  + 不同的端口，HTTP：80，HTTPS：443

  ![HTTPS过程图解](/images/https.png)

+ 过程：

  1. Client向Server发送请求
  2. Server生成一组密钥，包括一个公钥`crt public`（public key）和一个私钥`crt private`（private key）。然后将公钥`crt public`返回给Client。
  3. Client随机生成一个`key`，然后使用这个`crt public`对`key`加密，最后将加密后的`key`发送给Server。
  4. Server使用`crt private`对这个`key`解密，获得了`key`，然后将需要返回的网页内容使用这个`key`进行加密，最后将加密后的内容返回给Client。
  5. Client收到内容后使用`key`解密，获得了网页内容。

---

## 网页访问全过程

&emsp;&emsp;用户在浏览器中输入URL到响应页面的整过过程中，包含了许多计算机网络的协议。下面就简单介绍这个过程：

1. 本机获取IP地址
   + DHCP协议：使用UDP，将以太网帧广播，找到DHCP服务器，然后发送ACK确认收到信息。包括：本机IP、本地DNS服务器IP、first-hop路由的IP。
2. DNS解析
   + 发起DNS query：使用UDP，需要router的MAC地址。
   + ARP协议：通过广播获取router的MAC地址。router收到广播后发送ARP reply（含有router的MAC地址）
   + 回到DNS query，顺序为：DNS query发出$\rightarrow$本地域名服务器$\rightarrow$根域名服务器$\rightarrow$com顶级域名服务器$\rightarrow$获取解析后的IP地址
3. HTTP部分
   + 基于TCP，通过3次握手后，Client和Server建立链接，传送数据。

+ Details
  + DNS负载均衡：根据机器的负载量、该机器与用户地理位置的距离，返回合适的机器IP给用户，缩短响应时间
  + GET和POST
    1. 浏览器限制URL在2k个字节，服务器最多处理64k的URL，所以不能把过多的信息放在GET中的URL传送。
    2. 使用GET，同时在request body放数据，Server不一定会处理，视乎Server端的处理方式。
    3. GET方法发送一个数据包，POST方法发送两个数据包。具体情况是：GET方法将http的header和数据data一起发送出去，Server直接响应200；POST方法先发http的header，Server响应100（continue），然后Client再发送数据data，Server响应200。（FireFox只发送1次）。

---

## Reference

1. [HTTPS加密过程](<https://juejin.im/post/5a4f4884518825732b19a3ce>)
2. [TCP四次挥手中time_wait的作用](<https://blog.csdn.net/stpeace/article/details/75714797>)
3. [GET和POST的区别](<https://www.oschina.net/news/77354/http-get-post-different>)