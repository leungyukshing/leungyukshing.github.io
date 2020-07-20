---
title: LeetCode解题报告（103）-- 67. Add Binary
tags:
  - LeetCode
date: 2020-07-20 23:26:30
mathjax: true

---

## Problem

Given two binary strings, return their sum (also a binary string).

The input strings are both **non-empty** and contains only characters `1` or `0`.

<!-- more -->

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

**Constraints:**

- Each string consists only of `'0'` or `'1'` characters.
- `1 <= a.length, b.length <= 10^4`
- Each string is either `"0"` or doesn't contain any leading zero.

------

## Analysis

&emsp;&emsp;这道题目是二进制的加法。其实原理和十进制的加法是一样的，从低位开始计算，逢二进一。

------

## Solution

&emsp;&emsp;用除法来计算进位，用取余计算当前位的值。最后如果还有进位的话需要在开头多补一个1。

------

## Code

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        string res = "";
        int m = a.size() - 1, n = b.size() - 1, carry = 0;
        while (m >= 0 || n >= 0) {
            int p = m >= 0 ? a[m--] - '0' : 0;
            int q = n >= 0 ? b[n--] - '0' : 0;
            int sum = p + q + carry;
            res = to_string(sum % 2) + res;
            carry = sum / 2;
        }
        return carry == 1 ? "1" + res : res; // greatest position
    }
};
```

------

## Summary

&emsp;&emsp;数字加法的题目做了很多，实际上二进制和十进制的思路是一样的，把握好进位的做法，然后使用除法+取余的方法进行操作即可。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
