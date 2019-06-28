---
title: 使用Cheerio实现Nodejs爬虫
abbrlink: 12644
date: 2019-06-25 12:14:28
tags:
---

## Intro

&emsp;&emsp;爬虫是抓取网页数据最常使用的工具，我们可以通过爬虫抓取数据集，然后提供给深度学习的训练使用；也可以抓取网页数据，然后放到我们自己的网页上使用。在这篇博客我将介绍如何使用[cheerio](http://zetcode.com/javascript/cheerio/)来实现一个nodejs的爬虫。

<!-- more -->

---

## 实现

&emsp;&emsp;现在很多爬虫都使用python来做，但是python做的服务器数量还是比较少，一是因为python比较慢，二是因为使用python做服务器对程序员的要求比较高。对于学习而言，更多人选择的是nodejs，所以这里就分享如何在nodejs端做一个爬虫。

&emsp;&emsp;我们的任务是在中山大学数据科学与计算机学院的官网上抓取新闻信息，包括日期、标题以及对应的链接。

&emsp;&emsp;然后我们来简单地理解一下爬虫的原理，实质上他就是将网页的HTML抓取下来，然后通过一定的规则（根据class、id、标签等）去解析出我们需要的元素。

&emsp;&emsp;我们打开学院的网址：[中山大学数据科学与计算机学院](http://sdcs.sysu.edu.cn/)，我们要抓取的部分是【学院新闻】栏目的内容：

![cheerio1](/images/cheerio1.png)

&emsp;&emsp;查看网页元素，我们要的部分的代码在这里：

![cheerio2](/images/cheerio2.png)

&emsp;&emsp;然后我们要做的就是先把整个网页的html抓取下来，再根据对应的class和标签获取其中的内容就可以了。其中获取网页用的是`http`模块，解析网页用的是`cheerio`模块。解析的方法大概两个：

+ 获取标签内属性：`attr()`
+ 获取标签的内容：`text()`

完整代码（`test.js`）：

```javascript
var cheerio = require('cheerio');
var http = require('http');

var url = 'http://sdcs.sysu.edu.cn/';

// Get Data From the website
http.get(url, function(res) {

  var str = ""; // store data

  res.on('data', function(chunk) {
    str += chunk;
  });
  
  res.on('end', function() {
    let yearArr = getYear(str);
    let dateArr = getDate(str);
    let titleArr = getTitle(str);
    let linkArr = getLink(str);
    console.log(yearArr);
    console.log(dateArr);
    console.log(titleArr);
    console.log(linkArr);     
  });
});

function getYear(str) {
  var $ = cheerio.load(str);

  var arr = $(".dt-list-y")
  var dataTemp = [];
  arr.each(function(k, v) {
    var src = $(v).text();
    dataTemp.push(src);
  })
  //console.log(dataTemp);
  return dataTemp;
}

function getDate(str) {
  var $ = cheerio.load(str);

  var arr = $(".dt-list-md")
  var dataTemp = [];
  arr.each(function(k, v) {
    var src = $(v).text();
    dataTemp.push(src);
  })
  //console.log(dataTemp);
  return dataTemp;
}
function getTitle(str) {
  var $ = cheerio.load(str);

  var arr = $(".dt-list-title a")
  var dataTemp = [];
  arr.each(function(k, v) {
    var src = $(v).text();
    dataTemp.push(src);
  })
  //console.log(dataTemp);
  return dataTemp;
}

function getLink(str) {
  var $ = cheerio.load(str);

  var arr = $(".dt-list-title a")
  var dataTemp = [];
  arr.each(function(k, v) {
    var src = $(v).attr("href");
    dataTemp.push(src);
  })
  //console.log(dataTemp);
  return dataTemp;
}
```

## 结果

运行方式为：打开命令行，输入`node test`，回车。结果如下：

![cheerio3](/images/cheerio3.png)

## 小结

&emsp;&emsp;虽然现在基本上都是用python来做爬虫了，但是思路还是差不都的。所以借助这个简单的例子可以学习爬虫的使用。希望这篇博客能够帮助到您，谢谢！

