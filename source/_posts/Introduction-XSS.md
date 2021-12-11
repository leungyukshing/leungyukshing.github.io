---
title: 浅谈XSS攻击
mathjax: true
date: 2021-09-11 19:03:17
tags:
---

# Introduction

&emsp;&emsp;这会是我第二篇介绍安全相关的博客，上一篇介绍了CSRF，这次我将介绍一种更加普遍，也更加广泛的攻击——XSS。

<!-- more -->

# What is XSS

&emsp;&emsp;XSS的全称并没有X，而是Cross-Site Scripting（跨站脚本攻击），是一种代码注入攻击。XSS是OWASP中10大攻击漏洞的常客，已经存在很多年了，虽然开发者也想出了很多方法去防范XSS攻击，但是因为其种类多样，仍然很难完全防范。

&emsp;&emsp;XSS简单描述就是，攻击者通过在目标网站上注入恶意脚本，使之**在用户的浏览器上执行**，从而获取到用户的敏感信息，如Cookie、Session等。其本质是恶意的代码和正常的代码混在一起，浏览器无法区分，所以执行了恶意的代码。

# How XSS attack works?

&emsp;&emsp;为了了解XSS的过程，我们先来看一个例子：

&emsp;&emsp;一个搜索网站，用户输入关键词，通过URL参数传给服务端，然后在页面上展示这个关键词结果。这里就存在一个问题，页面html直接展示关键词的内容，那么攻击者就可以在这个关键词上做小动作。如果这个关键词不是输入纯文本，而是一段恶意代码的话，这样就能达到攻击的效果了。恶意代码可以是`<script>alert(123)</script>`，这样直接插入到html中，浏览器就会执行`alert(123)`，攻击成功。

&emsp;&emsp;试想一下，如果插入的代码片段不只是alert呢？它可以是一段包含逻辑的代码，比如获取用户的数据后，把数据发送到攻击者的服务器，这样一来，攻击者就可以获取到用户的敏感信息了。

&emsp;&emsp;那针对这种问题，要怎么解决呢？我们只需要把标签中的一些符号**转义**后再进行传输就可以了，这样在html页面展示的时候，就不要认为这是一个标签，而是一个字符串，页面中也会正常展示输入的内容。

# Summary

&emsp;&emsp;这篇博客从一个实际的例子简单解释了什么是XSS攻击，当然XSS攻击还要很多类型，后面有时间继续和大家分享。

# Reference

1. [前端安全系列（1）：如何防止XSS攻击](https://tech.meituan.com/2018/09/27/fe-security.html)
2. [csrf与xss攻击的详解与区别](https://blog.csdn.net/wuhuagu_wuhuaguo/article/details/104148444)
3. [OWASP Top Ten](https://owasp.org/www-project-top-ten/)