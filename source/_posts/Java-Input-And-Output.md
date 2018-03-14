---
title: Java常用输入输出
tags:
  - Java
abbrlink: 30112
date: 2018-03-14 13:36:19
---
&emsp;在初学java的时候，我们总是比较关心一个程序如何从命令行读取输出，如何输出数据。在这篇post，我就来给大家讲解一下关于java的一些常用输入输出操作。
<!-- more -->

## 输入
&emsp;在java中的输入同样和C++不一样，是使用本身提供的**Scanner类**来实现的。我们先来看一个简单的程序ComputeAreaWithConsoleInput.java：
```Java
import java.util.Scanner; // Scanner is in the java.util package

public class ComputeAreaWithConsoleInput {
  public static void main(String[] args) {
    // Create a Scanner object
    Scanner input = new Scanner(System.in);

    // Prompt the user to enter a radius
    System.out.print("Enter a number for radius: ");
    double radius = input.nextDouble();

    // Compute area
    double area = radius * radius * Math.PI;

    // Display result
    System.out.println("The area for the circle of radius " + radius + " is " + area);
  }
}
```

&emsp;从上面的程序可以看出，我们经常使用的是这条语句从命令行读入用户输入的数据：
> Scanner input = new Scanner(System.in);

&emsp;这条语句的意思是，创建了一个类型为Scanner的对象input。我们可以通过调用这个input的对象的各种方法进行读入数据。

Methods for Scanner Objects：

|   *Method*   | *Description*                                              |
| ------------ | ---------------------------------------------------------- |
|  nextByte()  | reads an integer of the **byte** type  |
|  nextShort() | reads an integer of the **short** type |
|  nextInt()   | reads an integer of the **int** type   |
|  nextLong()  | reads an integer of the **long** type  |
| nextFloat()  | reads a number of the **float** type   |
| nextDouble() | reads a number of the **double** type  |
|    next()    | reads a string that ends before **a whitespace character** |
|  nextLine()  | reads a line of text(i.e., a string ending with **the Enter Key** pressed) |


## 输出
&emsp;相比起输入，输出就稍微简单一下，上面程序中有两条输出语句，这就给出了我们常用的一个输出模板：
> System.out.print(String str);
> System.out.println(String str);

&emsp;其中，第一条的输出后是没有换行的，第二条输出后是有换行的。参数str是String类型的变量，可以直接用'+'连接。

&emsp;至此，java程序中最基本的输入输出语法就介绍完了，谢谢！
