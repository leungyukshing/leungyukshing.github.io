---
title: LeetCode解题报告（280）-- 1680. Concatenation of Consecutive Binary Numbers
tags:
  - LeetCode
mathjax: true
date: 2021-02-07 15:31:30

---

## Problem

Given an integer `n`, return *the **decimal value** of the binary string formed by concatenating the binary representations of* `1` *to* `n` *in order, **modulo*** `109 + 7`.

<!-- more -->

**Example 1:**

```
Input: n = 1
Output: 1
Explanation: "1" in binary corresponds to the decimal value 1. 
```

**Example 2:**

```
Input: n = 3
Output: 27
Explanation: In binary, 1, 2, and 3 corresponds to "1", "10", and "11".
After concatenating them, we have "11011", which corresponds to the decimal value 27.
```

**Example 3:**

```
Input: n = 12
Output: 505379714
Explanation: The concatenation results in "1101110010111011110001001101010111100".
The decimal value of that is 118505380540.
After modulo 109 + 7, the result is 505379714.
```

**Constraints:**

- `1 <= n <= 105`

------

## Analysis

&emsp;&emsp;这是一道二进制字符串拼接的问题，题目给出`n`，要求把`[1, n]`之间的数的二进制拼接起来，最后返回十进制结果。我们先看对于某个数字`k`，是怎么样拼接进去的。

1. 首先我们已经有了`[1, k - 1]`这个区间中所有数字拼接好的数，假设为`a`；
2. 然后我们需要把`k`插入进去，所以需要先把`a`左移`x`位，这个`x`是`k`的长度，然后把`k`加进去

&emsp;&emsp;所以问题就变成了这个`x`怎么求，如果每一个数都先把它从十进制转为二进制再求长度，效率会非常低下。这个`x` 实际上就是二进制串的长度，这个是有规律的。比如2的0次方是1，2的1次方是2，所以我们可以通过对比`k-1`和`k`就能够知道`x` 是否需要自增1。仅当需要进位的时候，这个`x`才会增加。

------

## Solution

&emsp;&emsp;判断`x` 是否需要自增的条件是`(i & (i - 1))`。还需要注意每次运算后都要进行模运算防止溢出。

------

## Code

```c++
class Solution {
public:
    int concatenatedBinary(int n) {
        const int kMod = 1e9 + 7;
        long ans = 0;    
        for (int i = 1, len = 0; i <= n; ++i) {
          if ((i & (i - 1)) == 0) ++len;
          ans = ((ans << len) % kMod + i) % kMod;
        }
        return ans;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是位运算的一道很好的应用题，这类题目主要是从二进制运算出发进行考察。这道题这道题目的分享到这里，谢谢您的支持！
