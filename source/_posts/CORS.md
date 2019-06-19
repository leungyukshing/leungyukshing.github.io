---
title: Vuejs+Golang解决CORS跨域问题
abbrlink: 15934
date: 2019-06-19 10:08:15
tags:
---

## 问题描述

&emsp;&emsp;在使用[Vuejs](https://vuejs.org/)+[beego](https://github.com/astaxie/beego)框架开发的过程中，前端本地测试的时候，在使用[axios](<https://github.com/axios/axios>)调用api获取服务器资源的时候出现了**跨域**的问题。

<!-- more -->

如图：![CORSIssue1](/images/CORSIssue1.jpg)

---

## 分析

&emsp;&emsp;从报错信息看，它说的是我从本地`https://localhost:8081`发起到`http://139.199.166.124:8080`的资源请求被CORS策略阻挡了。但是打开F12的Network里面是可以看到返回的数据的，那么这个CORS究竟是什么来的呢？

&emsp;&emsp;CORS（Cross Origin Resource Sharing），跨源资源共享，是浏览器基于安全性考虑的情况下，一种阻止不同域API的一种政策。它的目的有两个：一是确保网页发出的请求能够代表自己；二是阻止流氓js发出无效的请求。

&emsp;&emsp;可以看出CORS并不是一个错误，而是一个保护机制，但是这种保护机制对于本地测试不太友好。我们再来看看什么是Access-Control-Allow-Origin。这个是一个Http的请求头，一般由服务器端返回，它的值代表了服务器中的哪些域名是有权访问的，可以是：

+ `*`：允许所有的访问
+ 一个确定的域名。

&emsp;&emsp;那我们直接设为`*`就可以解决问题了吗？当然不是，如果我们的请求需要用到cookies认证，那么就不能设置为`*`，而要设置为经过安全认证的域名！同时还需要设置Access-Control-Allow-Credentials为True。

&emsp;&emsp;经过上面的分析，上面的错误就很明显了，因为服务器端没有给我一个允许`http://localhost:8081`访问的权限。（至于为什么是8081，是因为我在本地开了测试服务器占了8080，所以前端运行的端口是8081，而远程的服务器是8080端口），远程服务器允许的是`http://localhost:8080`访问，所以就出现问题了。

---

## 解决办法

&emsp;&emsp;因为是课程的项目开发，所以和后端的大佬是认识的，麻烦他改改就好了。

```go
//跨域
func (c *BaseController) AllowCross() {
    c.Ctx.ResponseWriter.Header().Set("Access-Control-Allow-Origin", "http://localhost:8080, http://localhost:8081")       //允许访问源
    c.Ctx.ResponseWriter.Header().Set("Access-Control-Allow-Methods", "POST, GET, PUT, OPTIONS")    //允许post访问
    c.Ctx.ResponseWriter.Header().Set("Access-Control-Allow-Headers", "Content-Type,Authorization") //header的类型
    c.Ctx.ResponseWriter.Header().Set("Access-Control-Max-Age", "1728000")
    c.Ctx.ResponseWriter.Header().Set("Access-Control-Allow-Credentials", "true")
    c.Ctx.ResponseWriter.Header().Set("content-type", "application/json") //返回数据格式是json
}
```

&emsp;&emsp;或者是本地测试完后，将前端端口改回8080就好了。

---

## 小结

&emsp;&emsp;跨域问题是前端开发过程中经常遇到的问题，一般的步骤先是用postman来确认api的正确性，然后打开浏览器的F12的Network查看网络包信息，最后根据报错信息找到问题。希望这篇博客能对你有帮助，谢谢！

## Reference

1. [CORS in wikipedia](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

2. [Vue跨域解决](https://www.cnblogs.com/lrj567/p/6141209.html)