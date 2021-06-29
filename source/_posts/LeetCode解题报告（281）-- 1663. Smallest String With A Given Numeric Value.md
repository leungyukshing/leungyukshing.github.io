---
title: LeetCode解题报告（281）-- 1663. Smallest String With A Given Numeric Value
tags:
  - LeetCode
mathjax: true
abbrlink: 6455
date: 2021-02-08 17:04:47
---

## Problem

The **numeric value** of a **lowercase character** is defined as its position `(1-indexed)` in the alphabet, so the numeric value of `a` is `1`, the numeric value of `b` is `2`, the numeric value of `c` is `3`, and so on.

The **numeric value** of a **string** consisting of lowercase characters is defined as the sum of its characters' numeric values. For example, the numeric value of the string `"abe"` is equal to `1 + 2 + 5 = 8`.

You are given two integers `n` and `k`. Return *the **lexicographically smallest string** with **length** equal to n and **numeric value** equal to k.*

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

<!-- more -->

**Example 1:**

```
Input: n = 3, k = 27
Output: "aay"
Explanation: The numeric value of the string is 1 + 1 + 25 = 27, and it is the smallest string with such a value and length equal to 3.
```

**Example 2:**

```
Input: n = 5, k = 73
Output: "aaszz"
```

**Constraints:**

- `1 <= n <= 105`
- `n <= k <= 26 * n`

------

## Analysis

&emsp;&emsp;题目给了两个数字，`n`是长度，`k`是总和。求出长度为`n`，总和为`k`的且字典序最小的字符串。因为要的是字典序最小的，这里直接用贪心就好了，不过要注意顺序是从后往前。原因是从前往后的话，先选小的后面可能无法凑够总和，但是从后往前的话，先选大的话，前面一定能凑好。

------

## Solution

&emsp;&emsp;因为长度`n`是确定的，都初始化为最小的`a`，然后，这样余额就变成了`k - n `，然后从后往前，如果余额比25大，直接选最大的`z`，循环结束后的result就是答案。

------

## Code

```c++
class Solution {
public:
    string getSmallestString(int n, int k) {
        k -= n;
        string result(n, 'a');
        
        for (int i = n - 1; i >= 0; i--) {
            int d = min(k, 25);
            result[i] += d;
            k -= d;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是贪心算法的一个简单应用，难度不大，主要是初始化讲究一点技巧。这道题这道题目的分享到这里，谢谢您的支持！
