---
title: gorm零值更新的坑
abbrlink: 29015
date: 2020-04-25 11:51:03
---

# gorm零值更新的坑

&emsp;&emsp;gorm是Go里面操作数据库比较常用的库，功能非常强大，接口支持也做得挺好，但是最近在使用的过程中碰到了一个坑，没注意的话还是挺难找出来的，特意在这里和大家分享。

<!-- more -->

&emsp;&emsp;踩坑的地方是在更新数据库的时候，我们先来看看gorm提供的几种更新的方式：

+ 更新所有的字段: `Save()`
+ 只更新改变的字段: `Update()`
+ 只更新选中的字段: `Updates()`

&emsp;&emsp;坑是在使用第三种更新方法的时候，我想更新一个数量为0，赋值后放到`Updates()`里面，然而**gorm并不会更新零值的字段**，它会忽略掉所有值为0的字段。所以解决的方法有以下两个：

1. 可以转为使用`Save()`，但需要注意这个会更新所有的字段
2. 使用map类型的结构，定义一个`map[string]interface{}`，`key`为数据库的列名，`value`是需要更新的内容，然后把这个map传入到`Updates()`中。