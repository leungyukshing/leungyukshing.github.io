---
title: React Native生命周期
abbrlink: 43849
date: 2020-04-20 11:08:18
tags:
---

## React Native生命周期

&emsp;&emsp;要想学会React Native编程，了解其组件的生命周期是很重要的。什么时候初始化组件，什么时候拉取数据，什么时候渲染，这些都与组件的生命周期密切相关。在这篇博客，我将简单地介绍React Native的生命周期。

<!-- more -->

## 生命周期

&emsp;&emsp;组件的几个重要生命周期顺序如下（调用次序自上而下）：

- `constructor()`
- `componentWillMount()`
- `render()`
- `componentDidMount()`

&emsp;&emsp;这里有一个很基本的概念就是：**数据只能加载到已经挂载的组件中**。接下来我将会分析我们在每一个周期一般会做什么。

### constructor

&emsp;&emsp;这是组件初始化最开始的阶段，此时组件还没有挂载到网页上。在这个阶段可以做state的**初始化**工作，但并不加载数据。

### componentWillMount

&emsp;&emsp;在这个阶段中，组件仍然没有被挂载，在这里调用`setState`方法并不会出发重渲染，所以也无法加载数据。这个方法比较少用。

### render

&emsp;&emsp;在这个方法，就是组件进行挂载的阶段，框架会生成真实的DOM节点，这里一般是定义页面的元素，并不会涉及到数据的加载。

### componetDidMount

&emsp;&emsp;这个方法是在组件已经完全挂载到页面上才会被调用，所以能够保证数据的加载。此外，在这个方法里面调用`setState`会触发重渲染，所以我们一般都会选择在这个函数中加载外部数据。

## 小结

&emsp;&emsp;了解React Native组件的生命周期对于coding有非常大的帮助，我们需要选择在正确的时间加载外部数据，否则就会出现页面加载后没有真实的数据。欢迎大家转发、评论，谢谢！
