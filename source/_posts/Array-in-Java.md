---
title: Java小知识之Array的基本用法
tags:
  - Java
abbrlink: 18177
date: 2018-03-15 22:45:28
---
## 前言
&emsp;在这篇post，我将讲述如何在Java中创建和使用**数组（array）**。
<!-- more -->

## 创建数组
&emsp;与C++的语法稍微不同，在Java中，我们使用以下的语法来创建一个数组：
```java
Type arrayRefVar[] = new Type[arraySize];
```

&emsp;这条语句创建了一个Type类型的数组，数组的首地址存放在变量**arrayRefvar**中，数组的大小是**arraySize**。

## 数组的操作
### 元素访问
&emsp;在Java中，数组元素的访问和C++是一样的，使用`[]`来对下标进行访问即可。

### 获取长度
&emsp;Java中的数组可以直接获取长度，可以使用以下语法来获取一个已申请空间的数组：
```java
int size = arrayRefVar.length;
```

### 遍历数组
&emsp;Java为用于提供了一个快捷而方便的方法去对一个数组进行全遍历，语法如下（这是一个对double类型数组的每一个元素输出的操作）：
```Java
for (double u: myList) {
  System.out.println(u);
}
```

### 数组的复制
&emsp;与C++一样，像`list2 = list1`这样的赋值语句会使得**list2**原本指向的内存空间变成垃圾，虽然JVM的GC机制最终会回收这一块垃圾，但是我依然不建议直接使用赋值的方法来对数组进行复制。
首先给出一种常用的方法来复制数组，这是与C++一样的：
```java
int[] sourceArray = {2, 3, 1, 5, 10};
int[] targetArray = new int[sourceArray.length];

for (int i = 0; i < sourceArray.length; i++) {
  targetArray[i] = sourceArray[i];
}
```
&emsp;除了上述的方法外，Java在`java.lang.System`给我们提供了`arraycopy()`方法对数组进行复制。语法如下：
```Java
arraycopy(sourceArray, src_pos, targetArray, tar_pos, length);
```
&emsp;将上面循环复制的代码替换一下：
```Java
System.arraycopy(sourceArray, 0, targetArray, 0, sourceArray.length);
```

&emsp;关于java数组的一些基本用法就讲到这里了，谢谢！
