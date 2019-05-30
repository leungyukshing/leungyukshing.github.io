---
title: pytorch的Upsampling函数参数问题
tags:
  - python
  - pytorch
abbrlink: 52635
date: 2019-05-22 00:57:11
---
## 问题
&emsp;&emsp;最近在跑代码的过程中无意间碰到了一个错误，是关于Pytorch中`nn.Upsampling()`的参数类型问题。现在许多人都会用这种写法，所以特意在这里分享一下解决的方法。
<!-- more -->

## 问题详情
**注意：Python版本为Python2.7**
&emsp;&emsp;首先看看报错的信息，如图：
![Pytorch upsampling问题](/images/upsampling1.png)
&emsp;&emsp;从报错信息中我们可以得知，这里的问题出在了`scale_factor`这个参数的类型。我们先来看看代码的写法：
```python
self.upsample = nn.Upsample(scale_factor=(1, 4, 4), mode='trillinear', align_corners=True)
```
&emsp;&emsp;这里就涉及到了`nn.Upsample()`这个函数，我们先来看看这个函数的源码。
![Upsampling源码](/images/upsampling2.png)
&emsp;&emsp;这样一看更慌了，因为这里的说明提到`scale_factor`参数是可以接受`tuple`的，但是现在传入`tuple`却报错了。不用着急，其实问题很简单，下面来解决。

## 解决办法
&emsp;&emsp;报错的地方主要是在使用`float()`类型转换的时候的问题。既然不能够一次过转换，那么我们对`tuple`中的每一个元素单独进行转换就好了。修改代码如下：
首先删除下面一行代码（Row 125）:
```python
self.scale_fac = float(scale_factor) if scale_factor else None
```
然后在Row 125添加如下代码：
```python
if isinstance(scale_factor, tuple):
  self.scale_factor = ()
  for factor in scale_factor:
    self.scale_factor += (float(factor),)
else:
  self.scale_factor = float(scale_factor) if scale_factor else None
```
&emsp;&emsp;这样就可以完美解决问题了，希望这篇博客能够帮助你！
