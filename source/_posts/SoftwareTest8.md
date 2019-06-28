---
title: 软件测试作业8
tags:
  - SoftwareTest
abbrlink: 37721
date: 2019-05-29 11:01:00
mathjax: true
---

## Assignment8

1. 构造NextDate问题的弱一般的等价类测试用例。
   - NextDate问题：NextDate()是整型变量month，day和year的函数，输入1812-2012年期间的某一日期的month，day和year的值，输出这一天的下一天的日期的month，day和year值。

<!-- more -->

------

## Answer

+ 弱/强、一般/健壮的等价类测试分类
  + 弱（weak）等价类测试
    + 针对单缺陷的等价类测试用例设计
    + 单缺陷：在同一输入条件下失效大概率由单个缺陷引起。
  + 强（strong）等价类测试
    + 针对多缺陷的等价类测试用例设计。
  + 一般（normal）等价类测试
    + 只覆盖有效等价类的测试用例设计。
  + 健壮（robust）等价类测试
    + 同时覆盖有效等价类和无效等价类的测试用例设计。
    + 健壮性：在异常情况下软件还能正常运行的能力。健壮性包括容错能力和异常恢复能力。容错性测试通常构造一些不合理的输入来诱导软件出错。

变量取值范围：

- C1: $1 \le month \le 12$
- C2: $1 \le day \le 31$
- C3: $1 \le year \le 2012$

等价类划分：

- M1 = {month: month has 30 days}
- M2 = {month: month has 31 days except December}
- M3 = {month: month is December}
- M4 = {month: month is February}
- D1 = {day: 1$\le$ day $\le$ 27}
- D2 = {day: day = 28}
- D3 = {day: day = 29}
- D4 = {day: day = 30}
- D5 = {day: day = 31}
- Y1 = {year: year is a leap year}
- Y2 = {year: year is a common year}

划分弱一般等价类：

- R1 = {M1, {D1, D2, D3}, {Y1, Y2}}
- R2 = {M1, D4, {Y1, Y2}}
- R3 = {M2, {D1, D2, D3, D4}, {Y1, Y2}}
- R4 = {M2, D5, {Y1, Y2}}
- R5 = {M3, {D1, D2, D3, D4}, {Y1, Y2}}
- R6 = {M3, D5, {Y1, Y2}}
- R7 = {M4, D1, {Y1, Y2}}
- R8 = {M4, D2, Y1}
- R9 = {M4, D2, Y2}
- R10 = {M4, D3, Y1}

&emsp;&emsp;**弱一般等价类测试用例覆盖**针对单缺陷，只覆盖有效等价类。

| Test Case | Year | Month | Day  | Expected Output | Note       |
| --------- | ---- | ----- | ---- | --------------- | ---------- |
| WN1       | 2016 | 4     | 28   | 2016.04.29      | 小月       |
| WN2       | 2016 | 4     | 30   | 2016.05.01      | 小天月跳转 |
| WN3       | 2017 | 3     | 28   | 2017.03.29      | 大月       |
| WN4       | 2017 | 3     | 31   | 2017.04.01      | 大月跳转   |
| WN5       | 2017 | 12    | 13   | 2017.12.14      | 12月       |
| WN6       | 2018 | 12    | 31   | 2019.01.01      | 年跳转     |
| WN7       | 2018 | 2     | 13   | 2018.02.14      | 2月        |
| WN8       | 2016 | 2     | 28   | 2016.02.29      | 闰月28     |
| WN9       | 2017 | 2     | 28   | 2017.03.01      | 非闰月跳转 |
| WN10      | 2016 | 2     | 29   | 2016.03.01      | 闰月跳转   |

## Reference

1. <https://www.jianshu.com/p/57cfcd11888e>
2. [Lec.16 by Prof. Guoyang Cai](/download/Lec16.pdf)
