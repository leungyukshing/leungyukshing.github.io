---
title: 软件测试作业4
tags:
  - SoftwareTest
mathjax: true
abbrlink: 38489
date: 2019-04-28 12:46:06
---

## 软件测试作业4

1. 计算下列代码片段的*Halstead*复杂度的11项内容：

   ```c
   if (month < 3) {
       month += 12;
       -year;
   }
   return dayray((int)(day + (month + 1) * 26 / 10 + year + year / 4 + 6 * (year / 100) + year / 400) % 7);
   ```

<!-- more -->

------

## Answer

### 难度度量元（*Halstead*复杂度）

- [*Halstead*复杂度（Maurice H. Halstead, 1977）](<https://en.wikipedia.org/wiki/Halstead_complexity_measures>)是软件科学提出的第一个计算机软件的分析“定律”，用以确定计算机软件开发中的一些定量规律。
  - *Halstead*复杂度采用一组基本的度量值，这些度量值通常在程序产生之后得出，或者在设计完成之后进行估算。
- *Halstead*复杂度根据程序中语句行的**操作符（operator）**和**操作数（operand）**的数量计算程序复杂度。
  - 操作符和操作数的量越大，程序结构就越复杂。
  - 操作符包括语言保留字、函数调用、运算符，也可以包括有关的分隔符等。
  - 操作数可以是常数和变量等标识符。
- *Halstead*方法的优点：
  - 不需要对程序进行深层次的分析，就能够预测错误率，预测维护工作量；
  - 有利于项目规划，衡量所有程序的复杂度；
  - 计算方法简单；
  - 与所用的高级程序设计语言类型无关。
- *Halstead*方法的缺点：
  - 仅仅考虑程序数据量和程序体积，不考虑程序控制流的情况；
  - 不能从根本上反映程序复杂性。

代码*Halstead*复杂度分析：

| Operator | Number of Occurrences |
| -------- | --------------------- |
| if       | 1                     |
| <        | 1                     |
| +=       | 1                     |
| -        | 1                     |
| return   | 1                     |
| dayray   | 1                     |
| int      | 1                     |
| +        | 6                     |
| *        | 2                     |
| /        | 4                     |
| %        | 1                     |

| Operand | Number of Occurrences |
| ------- | --------------------- |
| month   | 3                     |
| 3       | 1                     |
| 12      | 1                     |
| year    | 5                     |
| day     | 1                     |
| 1       | 1                     |
| 26      | 1                     |
| 10      | 1                     |
| 4       | 1                     |
| 6       | 1                     |
| 100     | 1                     |
| 400     | 1                     |
| 7       | 1                     |

经过统计，得到：

- 操作符（operator）个数$n_1 = 11$
- 操作数（operand）个数$n_2 = 13$
- 操作符出现总数$N_1 = 20$
- 操作数出现总数$N_2=19$

由上面的信息可以计算出如下的复杂度度量：

- *Halstead*程序词汇表长度（Program Vocabulary）：$n=n_1+n_2=11+13=24$

- *Halstead*程序长度或简单长度（Program Length）：$N=N_1+N_2 = 20 + 19 = 39$.

  注意到这里的$N$定义为*Halstead*长度，并非源代码行数。

- *Halstead*程序的预测长度（Calculated Program Length）：$N^\wedge = n_1log_2n_1 + n_2log_2n_2 = 11  \times log_211 + 13 \times log_213 = 86.1595$

  **key**：程序的实际长度$N$与预测长度$N^\wedge$非常接近，这表明即使程序还未编写完也能预先估算出程序的实际长度$N$。

- 程序体积或容量（Volumn）：$V=Nlog_2(n) = 39 \times log_2(24) = 178.8135$，这表明了程序在词汇上的复杂性。

- 程序级别（Level）：$L^{\wedge}=(\frac{2}{n_1}) \times (\frac{n_2}{N_2}) = \frac{2}{11} \times \frac{13}{19} = 0.1244$，这表明了一个程序的最紧凑形式的程序量与实际程序量之比，反映了程序的效率。

- 程序难度（Difficulty）：$D = \frac{1}{ {L^{\wedge} } } = \frac{1}{0.1244} = 8.0395$，这表明了实现算法的困难程度。

- 编程工作量（Effort）：$E=V\times D = \frac{V}{L^\wedge} = 178.8135 \times 9.0395 = 1437.5711$

- 语言级别：$L^{'} = L^\wedge \times L^\wedge \times V = 0.1244 \times 0.1244 \times 178.8135 = 2.7672$

- 编程时间（Hours）：$T^\wedge = \frac{E}{(S \times f)} = \frac{1437.5711}{60 \times 60 \times 18} = 0.0222$，这里$S = 60 \times 60$, $f = 18$。

- 平均语句大小：$\frac{N}{语句数} = \frac{39}{4} = 9.75$

- 程序中的错误数预测值：$B=\frac{B}{3000} = \frac{Nlog_2(n)}{3000} = \frac{178.8135}{3000} = 0.0596$

## Reference

1. https://en.wikipedia.org/wiki/Halstead_complexity_measures

2. [Lec.12 by Prof. Guoyang Cai](/download/Lec12.pdf)

