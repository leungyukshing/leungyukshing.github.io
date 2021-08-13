---
title: 浅谈JWT
mathjax: true
date: 2021-08-13 11:57:19
tags:

---

# Introduction

&emsp;&emsp;JWT，Json Web Token，是为了在网络应用环境中传递声明的一种开放标准（RFC 7519）.其设计目的是为了用于分布式站点的单点登录（SSO）。它本身是一种用来向服务器端认证请求者身份的手段，在这篇博客我将简单介绍一下JWT的原理及一些基本的使用场景。

<!-- more -->

# What is JWT?

&emsp;&emsp;JWT是一个开放标准，它定义了一种紧凑的、自包含的方式，用于作为JSON对象在各方之间安全地传输信息。该信息可以被验证和信任，因为它是数字签名的。

&emsp;&emsp;JWT由三部分组成，每个部分之间用`.`连接，这三部分是：

+ Header
+ Payload
+ Signature

## Header

&emsp;&emsp;Header由两部分信息组成：

+ type：声明类型，这里是jwt
+ alg：声明加密算法，通常会使用HMAC SHA-256

&emsp;&emsp;给个例子：

```
{
	"type": "JWT",
	"alg": "HS256"
}
```

&emsp;&emsp;然后会对这个头部信息进行base64编码，结果就是：`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`这样一串字符串。

## Payload

&emsp;&emsp;Payload是存放真正有效载荷的地方，它包含声明（Claim），声明有三种类型：

+ registered claim：标准中注册的声明，这些是RFC标准中预定义好的声明，它们不是必须带上的，但是是推荐使用；
+ public claim：公共的声明；
+ private claim：私有的声明

### Registered Claim

+ iss：JWT的签发者
+ sub：JWT所面向的用户
+ aud：接受JWT的用户
+ exp：JWT的过期时间，这个时间必须大于签发时间
+ nbf：定义在什么时间之前，这个JWT是不可用的
+ iat：JWT的签发时间
+ jti：JWT的唯一身份标识，作为一次性token从而回避重放攻击

### Public Claim

&emsp;&emsp;这个部分可以添加任何信息，一般是添加用户相关或业务相关的信息，但不建议添加敏感信息，因为这个部分的内容在客户端可以解密。

### Private Claim

&emsp;&emsp;这部分是JWT提供者和接收者共同定义的声明，也不建议存放敏感信息。



&emsp;&emsp;总的来说Payload部分就是把结构化的信息（可以按照标准，也可以自己定义）base64加密后，作为JWT第二部分的内容，这部分的内容相当于是明文的。

## Signature

&emsp;&emsp;JWT的第三部分信息是签名，签名也是由三个部分组成：

+ header（base64后的字符串）
+ payload（base64后的字符串）
+ secret：这个是存储在服务端的，它是用来进行JWT的签发和验证，相当于服务端的私钥，因此这个绝对不能泄露，否则JWT就可以被客户端签发了。

# Why JWT?

&emsp;&emsp;看完了JWT基本的组成部分和怎么生成后，我们就要来看为什么需要有JWT，什么场景下需要使用JWT？

## session认证

&emsp;&emsp;在一个最基本的需要用户登录的应用中，存在一种场景：用户第一次登录，然后进行某些操作。后续的每次操作服务端都要验证用户的请求，但是HTTP协议是无状态的，而又不可能每次请求都需要用户重新输入身份信息，于是就有了session这个东西。

&emsp;&emsp;在用户第一次登录系统是，服务端会生成一个session，并记录好session和这个用户的关系，然后把这个session传给客户端，客户端会保存为cookie，客户端在后续的请求中，都把这个cookie带上，这样对于后续的请求，服务端就知道是哪个用户在操作了，并且也可以根据session来验证是否合法的请求。

&emsp;&emsp;这种方法看上去能够到一定程度上解决问题，但是随着用户数量的增加，它的缺点也变得非常明显：

1. Session存储：每一个用户登录都需要在服务端存储一份session信息，如果是一个广泛使用的应用，那么服务端需要的存储资源将是非常大的；同时对于同一个用户多设备的场景，可能还要存多份，所以对于存储来说是一个挑战；

2. CSRF：因为服务端是完全基于session来进行用户识别，如果这个session被截获，那么就很容易受到CSRF攻击，原因是浏览器请求时会自动带上cookie。

## token认证

&emsp;&emsp;JWT是token认证的典型代表，和session最大的不同点是，服务端不再需要存储用户的登录信息。而是在用户第一次成功认证后，服务端生成一个token返回给客户端，此时客户端会把这个token存在local storage中，之后每次请求都在Header中携带上这个token，服务端收到后会验证这个token，通过验证才处理请求。

&emsp;&emsp;使用token的好处就是能够避免CSRF攻击，因为浏览器请求并不会自动把token带上，这里就能解决一部分的安全问题。但是仍有一些安全问题是需要注意的：

1. 在header和payload中不能带有敏感信息，因为这两部分实际上都是json字符串经过base64编码，相当于明文，客户端是能够解开的，只有signature部分是加密的；
2. signature的部分安全性取决于secret，所以服务端不能泄露这个secret；
3. token需要有过期时间，因为这个token在http传输过程中也是会有被截取的风险，所以应该有一个合理的过期时间。现在一般的做法是设置一个比较短的过期时间，然后给用户一个`refresh_token`，一定时间后，再使用这个`refresh_token`去换取新的token。

---

# Reference

1. [Introduction to JSON Web Tokens](https://jwt.io/introduction)
2. [JWT（Json Web token）认证详解](https://www.cnblogs.com/lizm166/p/7844110.html)
3. [一文讲解JWT用户认证全流程](https://zhuanlan.zhihu.com/p/158186278)
4. [JWT详解](https://www.jianshu.com/p/4a124a10fcaf)
5. [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)
6. [为什么要使用token，token与session区别是什么](https://www.codenong.com/cs105551783/)
7. [cookie和token都放在header中，为什么不会劫持token](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/31)