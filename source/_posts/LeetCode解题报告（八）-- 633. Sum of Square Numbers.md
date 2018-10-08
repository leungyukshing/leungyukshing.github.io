---
title: LeetCode解题报告（八）-- 633. Sum of Square Numbers
tags:
  - LeetCode
abbrlink: 9055
date: 2018-09-17 16:59:25
mathjax: true
---
## Problem
Given a non-negative integer `c`, your task is to decide whether there're two integers `a` and `b` such that $a^2 + b^2 = c$.
<!-- more -->

**Example 1:**
```
Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
```

**Example 2:**
```
Input: 3
Output: False
```

## Analysis
&emsp;&emsp;这是一题关于数学的算法题，要求的是一个数是否能够由两个数平方的和组成。难度本身不大，我们只需要从0 到 $\sqrt c$中找出两个满足要求的`a`和`b`，但是想要找到快速的方法却很具技巧性。这里不讨论使用两个循环穷举的方法，复杂度为$O(n^2)$。
&emsp;&emsp;我第一个想法是使用减法的思想，即$a^2 = c - b^2$，两头随便取一头开始遍历，假设循环变量为`i`，那么对于每个$c - c^2$，都判断一下是否可以完全开根号，如果可以，则return `true`。这个算法本身复杂度也不高，是$O(n)$。
&emsp;&emsp;从第一个想法中，可以发现，我们的遍历方向是任意的，即从0到$\sqrt c$和从$\sqrt c$到0是一样的，对判断结果没有影响，那么我们可以两头都移动。这个算法的优势在于，对于那些**不能由两个平方数组成的数**，我们可以从两端同时查找，因此得出答案的速度会更加快，时间大概是方法一的一半。

## Solution
&emsp;&emsp;我们首先求出$\sqrt c$，使用整数存储（相当于向下取整）。然后就开始遍历，假设左游标是`a`，右游标是`b`：
  + 若$a^2 + b^2 == c$，return `true`;
  + 若$a^2 + b^2 < c$，a++;
  + 否则，b--;
**注意**：`a`从0开始，要包含c本身就是平方数的情况！

## Code
```C++
class Solution {
public:
    bool judgeSquareSum(int c) {
        int sqrtOfC = sqrt(c);
        int i = 0;
        while (i <= sqrtOfC) {
            if (i*i + sqrtOfC * sqrtOfC == c)
                return true;
            else if (i*i + sqrtOfC * sqrtOfC < c)
                i++;
            else
                sqrtOfC--;
        }
        return false;
    }
};
```
**运行时间：**约0ms，超过97.06%的CPP代码。

## Summary
&emsp;&emsp;这虽然是一题简单的关于平方数的数学题，但是如果自己思考的话，会发现不同算法的效率相差是很大的。在面对线性数组问题，通常我们降低算法复杂度的方法有：（1）二分；（2）首尾一起移动。这道题的解析就到这里了，谢谢你的支持！
