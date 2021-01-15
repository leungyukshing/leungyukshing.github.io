---
title: 浅谈OAuth
tags:
mathjax: true
date: 2021-01-1 23:29:11

---

## What is OAuth?

&emsp;&emsp;OAuth是当前最流行的**授权机制**，它本质要解决的问题是：**允许用户（User）向第三方应用（Client）授权，允许第三方应用（Client）访问用户的数据（Resource）**。

------

## Glossory

- **User**：用户，资源的所有者；
- **Client**：第三方应用（Third-party Application）；
- **Server**：服务器，这里是资源服务器（Resource Server）和认证服务器（Authorization Server）的统称；
- **Resource**：用户数据，存放在资源服务器上；
- **Token**：访问凭证，Client用来访问Server的凭证。

------

## Why OAuth?

&emsp;&emsp;在这一部分我将用一个例子在帮助大家理解OAuth的特别之处。场景如下：

&emsp;&emsp;用户（User）把自己的照片（Resource）存放在了网盘（Server）上，有一天它想把这些照片发到一个社交平台（Client）上，这个时候社交平台就需要到网盘去获取用户的照片，这里存在着授权认证的问题。

### Before OAuth

&emsp;&emsp;在介绍OAuth前，我们来看看没有OAuth前这种场景是如何解决的。用户（User）登陆网盘是通过账户/密码的形式进行认证，所以如果社交平台（Client）想要访问用户的照片，它就需要获得授权，这里授权的方法是：**用户（User）在社交平台（Client）上输出自己的账号密码，然后社交平台拿着用户的账号密码去访问网盘拿到图片。**这个方法有什么问题呢？

1. 用户（User）的账号和密码都暴露给了社交平台（Client），如果社交平台存储了用户的账号和密码，那么它随时都可以去访问用户在网盘（Server）里所有的数据（Resource），社交平台的权限就相当于用户本身，**用户的数据会有泄漏的风险，而且对访问权限没有办法控制**；
2. 如果社交平台不存储账号和密码，那么用户每次想在社交平台发照片，就需要重新输入账号密码，**用户的操作会很不方便**；
3. 如果用户不想这个社交平台获取自己的信息，或者它认为这个社交平台会有泄露信息的风险，想要终止使用，那么它就必须到网盘中改密码（和自己登陆的密码是同一个），**如果其他平台想要读取网盘的信息，也要一并修改**；

### With OAuth

&emsp;&emsp; 在各种互联网第三方应用兴起的时候，上面的这些问题变得异常突出，用户数据的安全性迎来了很大的考验。OAuth应运而生，在一定程度上解决了用户数据授权的安全性和便利性的问题。我们先来看看有了OAuth后，上面的几种场景将会如何解决。

1. 用户（User）的账号和密码不需要交给社交平台（Client）。用户（User）通过在服务器（Server）认证后，交给社交平台（Client）一个访问凭证（Token），而且这个凭证有访问范围和时间的限制，这样社交平台（Client）对用户数据（Resource）的访问就是可以控制的；
2. 社交平台（Client）没有实际获得用户的账号和密码，它拥有的只是一个访问凭证（Token），而且这个凭证是有时间限制的，在有效期内（如7天内）是有效的，不需要用户（User）重复授权认证；
3. 用户（User）终止社交平台（Client）的访问是非常简便的，一个是访问凭证有时间有效期，用户不再授权后，一段时间就会过期；另一方面，如果用户发现社交平台有泄露个人信息的风险，他可以立即通过服务器取消访问凭证。

------

## How OAuth works?

&emsp;&emsp;既然OAuth已经深入到我们的日常生活中，那我们就来了解一下它的主要的工作机制，它是如何把授权给独立出来进行管理的，又是如何分发访问凭证的。

![OAuth](https://www.ruanyifeng.com/blogimg/asset/2014/bg2014051203.png)

&emsp;&emsp;根据上面的流程，我将对每一步进行解释：

- A：客户端（Client）向用户（User）请求授权访问用户的个人数据；
- B：用户（User）同意客户端访问自己的数据；
- C：客户端（Client）拿到用户（User）的授权后，向服务器（Server）申请访问凭证（Token）；
- D：服务器对客户端认证后，分发访问凭证（Token）；
- E：客户端（Client）使用访问凭证（Token）向服务器（Client）获取用户数据；
- F：服务器（Server）对请求中的访问凭证（Token）进行校验，返回用户数据。

&emsp;&emsp;上面几个步骤中，最关键的就是B和C，用户是怎么样安全地通过客户端，告诉服务器自己的授权呢？OAuth支持多种模式的授权，其中最重要、最普遍使用的就是**授权码模式**。

### 授权码模式（AuthCode）

&emsp;&emsp;授权吗模式是**功能最完整、流程最严密**的授权模式。它通过客户端的后台服务器，与服务器进行认证互动。

### 简化模式

&emsp;&emsp;在简化模式下，用户不需要通过第三方应用的服务器去进行授权，直接跳过了授权码。用户可以直接在浏览器中向服务器申请访问凭证，在这种情况下，访问凭证对用户是可见的。

### 密码模式

&emsp;&emsp;在密码模式下，就有点fallback到OAuth之前的情况了。用户把密码输入到第三方应用的客户端，由客户端向用户承诺不存储用户的密码。在这种情况下，一般这个第三方应用是能够高度信任的，否则风险非常大。

### 客户端模式

略

## Reference

1. [What is OAuth](https://www.csoonline.com/article/3216404/what-is-oauth-how-the-open-authorization-framework-works.html)
2. [What is SSO](https://www.csoonline.com/article/2115776/what-is-sso-how-single-sign-on-improves-security-and-the-user-experience.html)
3. [2FA eexplained](https://www.csoonline.com/article/3239144/2fa-explained-how-to-enable-it-and-how-it-works.html)
4. [OAuth1.0--RFC 5849](https://tools.ietf.org/html/rfc5849)
5. [OAuth2.0--RFC 6749](https://tools.ietf.org/html/rfc6749)
6. [理解OAuth2.0](https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
7. [OAuth2.0的一个简单解释](
