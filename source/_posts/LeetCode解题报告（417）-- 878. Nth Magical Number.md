---
title: LeetCode解题报告（417) -- 878. Nth Magical Number
mathjax: true
date: 2021-12-12 17:00:24
---

## Problem

A positive integer is *magical* if it is divisible by either `a` or `b`.

Given the three integers `n`, `a`, and `b`, return the `nth` magical number. Since the answer may be very large, **return it modulo** `109 + 7`.

<!-- more -->

**Example 1:**

```
Input: n = 1, a = 2, b = 3
Output: 2
```

**Example 2:**

```
Input: n = 4, a = 2, b = 3
Output: 6
```

**Example 3:**

```
Input: n = 5, a = 2, b = 4
Output: 10
```

**Example 4:**

```
Input: n = 3, a = 6, b = 4
Output: 8
```

**Constraints:**

- `1 <= n <= 109`
- `2 <= a, b <= 4 * 104`

------

## Analysis

&emsp;&emsp;题目给了magic number的定义是能够整除`a`或`b`的数字，要求找出第N个。这类题目一般有两种解法，一种是利用程序的思想进行搜索，另一种是数学方法。从解题的角度，数学方法确实很难想到，这里我就分享程序的方法。

&emsp;&emsp;找第N个数字的问题不妨先放一边，先看给定一个`x`，我们能知道它里面有多少个magic number。能够整除`a`的数字个数是`x / a`个，能够整除`b`的数字个数是`x / b`个，但是这中间有重复。比如`x = 24, a = 3, b = 4`，其中12和24都是能够同时被3和4整除的，所以总的数量应该是`x / a + x / b`还有减去重复的数量。重复的数量就是`x`除以`a`和`b`的最小公倍数。所以总结一下，当我们知道`x`后，它里面就有`x / a + x / b + x / lcm(a, b)`个magic number，而它自己就是第N个数字了。

&emsp;&emsp;有了一个限制条件，题目也给出了`x`的取值范围，在这两天提示下我们就要往**二分答案**上思考了。采用二分搜索，对于每一个`mid`按照上面的公式计算出数量，如果大于N，则说明`mid`选大了，需要去左半部分寻找；反之则到右半区间搜索。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int nthMagicalNumber(int N, int A, int B) {
        int MOD = 1e9 + 7;
        long lcm = A * B / gcd(A, B), left = 2, right = 1e14;
        
        while (left < right) {
            long mid = left + (right - left) / 2;
            if (mid / A + mid / B - mid / lcm < N) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left % MOD;
    }

    int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较复杂的二分答案，主要难点在于如果构建出限制条件。需要分别计算出`x / a`和`x / b`，还要减去重复的部分。这道题目的分享到这里，感谢你的支持！
