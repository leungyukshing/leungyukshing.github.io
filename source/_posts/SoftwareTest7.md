---
title: 软件测试作业7
tags:
  - SoftwareTest
abbrlink: 38681
date: 2019-05-29 10:50:49
---

## Assignment7

1. 构造下述三角形问题的弱健壮的等价类测试用例。
   - 三角形问题：输入三个不超过100的正整数作为三角形的三条边，判断三角形是等边三角形、等腰不等边三角形、完全不等边三角形还是不能构成三角形。

<!-- more -->

------

## Answer

+ 等价类的划分
  + 一个输入等价类是指程序输入域的某个子集，在该子集中，各个输入数据对于揭露程序中的错误是等效的。测试某一个等价类的代表值就等同于对这个等价类的其他值的测试。
  + 等价类的划分有两种：
    + 有效等价类：对于程序规格说明来说是合理的、有意义的输入数据构成的集合。
    + 无效等价类：对于程序规格说明来说是不合理的、无意义的输入数据构成的集合。

### 第一步：划分等价类

- R1 = {<*a*,*b*,*c*>: the triangle with sides *a*, *b*, and *c* is equilateral}
- R2 = {<*a*,*b*,*c*>: the triangle with sides *a*, *b*, and *c* is isosceles}
- R3 = {<*a*,*b*,*c*>: the triangle with sides *a*, *b*, and *c* is scalene}
- R4 = {<*a*,*b*,*c*>: sides *a*, *b*, and *c* do not form a triangle}

### 第二步：为每个等价类构造测试用例

| Test Case | a    | b    | c    | Expected Output |
| --------- | ---- | ---- | ---- | --------------- |
| WN1       | 3    | 3    | 3    | Equilateral     |
| WN2       | 3    | 3    | 4    | Isosceles       |
| WN3       | 3    | 4    | 5    | Scalene         |
| WN4       | 1    | 1    | 2    | Not a triangle  |

### 第三步：构造弱健壮性等价类测试用例

因为*a*,*b*,*c*的非法值，所以要增加弱健壮等价类测试用例

| Test Case | a    | b    | c    | Expected Output              |
| --------- | ---- | ---- | ---- | ---------------------------- |
| WR1       | -1   | 3    | 3    | value of *a* is out of range |
| WR2       | 3    | -1   | 3    | value of *b* is out of range |
| WR3       | 3    | 3    | -1   | value of *c* is out of range |
| WR4       | 110  | 98   | 99   | value of *a* is out of range |
| WR5       | 98   | 110  | 99   | value of *b* is out of range |
| WR6       | 98   | 99   | 110  | value of *c* is out of range |
| WR7       | 3.5  | 3    | 4    | value of *a* is not type int |
| WR8       | 3    | 3.5  | 4    | value of *b* is not type int |
| WR9       | 3    | 4    | 3.5  | value of *c* is not type int |

------

## Reference

1. <http://www.testingeducation.org/conference/wtst3_collard4.pdf>
2. [Lec.16 by Prof. Guoyang Cai](/download/Lec16.pdf)