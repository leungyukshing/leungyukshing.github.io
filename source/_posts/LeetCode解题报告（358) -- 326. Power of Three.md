---
title: LeetCode解题报告（358) -- 326. Power of Three
tags:
  - LeetCode
mathjax: true
date: 2021-05-02 11:53:36

---

## Problem

Given an integer `n`, return *true if it is a power of three. Otherwise, return false*.

An integer `n` is a power of three, if there exists an integer `x` such that $n == 3^x$.

<!-- more -->

**Example 1:**

```
Input: n = 27
Output: true
```

**Example 2:**

```
Input: n = 0
Output: false
```

**Example 3:**

```
Input: n = 9
Output: true
```

**Example 4:**

```
Input: n = 45
Output: false
```

**Constraints:**

- `-231 <= n <= 231 - 1`

 

**Follow up:** Could you solve it without loops/recursion?

------

## Analysis

&emsp;&emsp;题目给出一个数字`n`，要判断这个数是否3的`x`次方。最无脑的方法就是写个循环，不断除以3，最后如果剩下1的话就说明这个数是3的`x`次方。但这里我们要看看能否在不用递归或者循环的前提下，快速判断出结果。

&emsp;&emsp;在数学中，解决次方很常见的做法是使用对数，因为对数有个换底公式$\log a^b = \frac{\log 10^b}{\log 10^a}$，这里我用了10作为底数（实际上换成什么底数都是成立的）。所以如果题目给出的`n`满足$n = 3^x$，则会有$\log 3^n = \log 3^{3^x} = x$为整数，于是题目转化为判断$\log3^n$是否为整数。

&emsp;&emsp;使用换底公式，$log3^n = \frac{\log10^n}{\log10^3}$，运算出来之后，判断是否整数即可。注意这里使用10为底的原因是，C++存在精度问题，如果使用自然数或2为底，将得不到正确的答案。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        return (n > 0 && int(log10(n) / log10(3)) - (log10(n) / log10(3)) == 0);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目更偏向于数学知识，主要是使用对数的性质处理次方问题。同时也涉及到了C++计算过程中，判断整数、精度等问题，非常值得总结。这道题目的分享到这里，感谢你的支持！
