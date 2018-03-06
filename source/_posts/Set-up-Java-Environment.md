---
title: 搭建Java编译及运行环境
tags:
  - Java
abbrlink: 49360
date: 2018-03-06 13:25:55
---
## Introdution
  从这篇post开始，我将逐步介绍java的一些内容。我是利用了寒假的时间粗略地学习了java的一些知识，希望通过介绍自己的学习经历，能让大家对java有个大概的认识。今天，我们先来讲解一下如何在自己的PC上配置java的开发环境

## Java运行机制
首先，所有的源代码(source code)必须先写在一个后缀名为.java的文件中，这跟C++是类似的。
然后，我们需要使用java编译器把这些源代码文件编译成.class文件，这些.class文件中包含了能被**Java Virtual Machine**识别的二进制代码。
最后，**Java Launcher Tool**通过运行**JVM**的一个实例来运行你的程序
![Java代码执行简图](/images/java1.png)

### 详解JVM
Java Virtual Machine, 中文称为Java虚拟机，是一种能够运行Java bytecode的虚拟机。由于JVM屏蔽了与具体操作系统平台相关的信息，使得Java程序只需要生成在JVM上运行的目标代码(可以简单理解为.class文件)，就可以在多种平台上不加修改地运行。
![](/images/java2.png)

## 配置环境
### 1.下载JDK
首先我们下载java开发工具包JDK,参考网址：http://www.oracle.com/technetwork/java/javase/downloads/index.html
选择适合的版本进行下载并安装，根据指示就可以了。
### 2.配置环境变量
JDK成功安装后，右键“此电脑”， 点击“属性”，在左栏点击“高级系统设置”， 然后点击“环境变量”进入环境变量设置界面。
然后在“系统变量”设置如下参数，若已存在则“编辑”，若不存在则“新建”：
  + 变量名：JAVA_HOME
  + 变量值：C:\Program Files\Java\jdk-9.0.4(修改成自己的路径)
  + 变量名：CLASSPATH
  + 变量值：.;C:\Program Files\Java\jdk-9.0.4\lib(注意不要漏掉前面的.;)
  + 变量名：Path
  + 变量值：C:\Program Files\Java\jdk-9.0.4\bin

至此，在你的电脑上就已经配置好了Java的开发环境。我们现在来检测一下，打开cmd输入以下命令：
> java --version
如果有版本信息显示，则说明安装成功
![版本信息](/images/java3.png)

现在我们就可以来写第一个java程序啦！
