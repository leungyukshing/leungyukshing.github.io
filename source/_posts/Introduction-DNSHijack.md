---
title: 浅谈DNS劫持
mathjax: true
date: 2021-08-26 20:01:17
tags:
---

# Introduction

&emsp;&emsp;今天我们来聊DNS劫持。主要看一下DNS劫持是怎么发生的，已经它带来了什么问题。

<!-- more -->

# What is DNS Hijack?

&emsp;&emsp;要搞清楚DNS劫持，首先要知道一个正常的网络请求的步骤：

1. 客户端根据域名，向服务器发起请求；
2. 客户端先向DNS服务器获取解析结果，通过域名换取目标服务器的IP地址；
3. 之后再根据IP地址发起请求（再后面的这里就不详细说了）

&emsp;&emsp;而DNS劫持就发生在第二步，DNS劫持是指劫持了域名解析的回包，也就是说客户端拿不到DNS服务器返回的正确的解析结果，而是被调包了。

# How DNS Hijack works?

&emsp;&emsp;DNS劫持怎么发生呢？最直接的手段就是入侵DNS服务器，获取解析记录控制权，当有用户请求某个域名解析的时候，把返回的IP地址替换为恶意的IP，这样用户就访问到了恶意的网站了，在恶意网站上就可以盗取用户数据，传播病毒等。

&emsp;&emsp;特别注意，DNS的查询请求一般走的是UPD协议，所以没有很强的认证机制，请求包非常好篡改。所以还有一种方法是，直接对UDP端口53（DNS协议使用的是53端口）上的DNS查询进行侦听，利用规则匹配的方式，当发现满足规则时，立即伪装成目标域名的解析服务器返回虚假查询结果（这个也是GFW的工作原理之一）。

# How to defend?

&emsp;&emsp;面对DNS劫持，我们有什么方法呢？对于DNS解析服务器提供商而言，当然是要加强防护，包括软件和硬件上的防护；而对个人用户来说，我们可以切换DNS服务器，通过指定DNS服务器，绕开一些有问题的DNS服务器。

# Reference

1. [DNS Hijacking](https://www.imperva.com/learn/application-security/dns-hijacking-redirection/)