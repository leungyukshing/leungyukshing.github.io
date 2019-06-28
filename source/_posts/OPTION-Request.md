---
title: axios之PUT请求问题总结
abbrlink: 6991
date: 2019-06-19 10:46:11
tags:
---

## 问题描述

&emsp;&emsp;在测试PUT请求api的时候，遇到了很奇怪的405(Method Not Allowed)错误，一下子就蒙了。后端明明写了接受PUT请求，但是却报了405错误。

<!-- more -->

服务端LOG信息：

![login_option_backend](/images/login_option_backend.png)

---

## 分析

&emsp;&emsp;我们还是按照老方法，查看F12的Network，然后就发现了一些奇怪的事情。虽然我只是用axios发起了一个PUT请求，但是Network却有两个request：一个是OPTIONS，第二个才是PUT。这个时候就恍然大悟了，后端拒绝了OPTIONS，所以PUT的请求自然也没有处理了。那么这个多出来的OPTIONS又是什么来的呢？

&emsp;&emsp;首先我们来看看Request。Request分类两种：简单请求和预检请求。

+ **简单请求**：满足下列条件
  + 使用下列请求方法之一：
    + GET
    + HEAD
    + POST
  + HTTP的Header仅包含下面的字段：
    + Accept
    + Accept-Language
    + Content-Language
    + Last-Event-ID
    + Content-Type：application/x-www-form-urlencoded、multipart/form-data、text/plain
+ **复杂请求**：不满足上述条件的都是复杂请求，如我们这里提到的PUT。

&emsp;&emsp;总结一下，就是我们在前端使用了复杂请求PUT，axios发送了两个Request：OPTIONS和PUT。而后端无法响应OPTIONS，所以就出错了。

---

## 解决方法

&emsp;&emsp;解决的思路有两个方向：一个是将预检请求转化为简单请求；二是在后端修改代码，接受OPTIONS请求。因为前端的请求包括了很多的预检请求，所以第二种方法比较合适。在后端加上对应的相应代码就可以了。

```go
response.addHeader("Access-Control-Allow-Methods", "POST,GET,OPTIONS,PUT,DELETE");
```

修改后刷新页面，在Network中可以看到两个成功的Request。

![user_option](/images/user_option.png)

![user_put](/images/user_put.png)

---

## 小结

&emsp;&emsp;实话说自己之前是不清楚OPTION请求是有什么用的，通过这次解决PUT请求，也让我了解了相关的知识。OPTION可以理解为是一个预先询问，在确保能够访问后再使用PUT等方法进行数据传输，可以看作为是一种保险机制。希望这篇博客能够帮助到您，谢谢！

---

## Reference

1. [axios接口发起两次请求](https://blog.csdn.net/qq_27626333/article/details/77005911)
2. [Why OPTIONS?](https://cloud.tencent.com/developer/article/1046663)