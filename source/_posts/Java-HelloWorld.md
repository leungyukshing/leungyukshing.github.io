---
title: 第一个Java程序
tags:
  - Java
abbrlink: 9170
date: 2018-03-10 00:18:10
---
## 第一个Java程序
&emsp;在这篇post中，我将讲述如何写第一个Java程序--Welcome.java。首先我们要对Java这门语言有一定的语法了解。
&emsp;每一个Java程序，都必须**含至少1个class**，这是JVM运行的机制所要求的，并且这个类的类名必须和文件名相一致，只有这样，JVM才能找到这个类。程序的入口和C族语言一样，依然是**main()函数**，这个函数是作为**public**且**static**的方法被JVM所识别。
<!-- more -->

&emsp;Java中的**输入输出**相比起C++会稍微复杂，这里我先讲解输出。我们常用的语法是
```Java
System.out.println(args); // 输出后含换行
System.out.print(args); // 输出后不含换行
```

到这里，我们就有能力写出第一个简单的Java程序了。以下给出代码
```C++
public class Welcome {
  public static void main(String[] args) {
    // Display message Welcome to Java! to the console
    System.out.println("Welcome to Java!");
  }
}
```

将代码保存在一个名字为Welcome.java的文件中，打开命令行，输入命令：
> javac Welcome.java
> java Welcome

这样就可以看到在命令行中有"Welcome to Java!"的字样了！

最后再补充一下Java常用的命名规范：
  + 类使用Pascal命名方法(例如 Welcome, Animal);
  + 成员方法、变量等使用Camel命名法(例如 getName(), int newName);
  + 用于测试一个类的测试类通常以Demo结尾(例如 AnimalDemo, StudentDemo);

这里先讲这么多，以后会陆续补充，谢谢！
