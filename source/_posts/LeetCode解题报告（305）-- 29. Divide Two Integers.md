---
title: LeetCode解题报告（305)-- 29. Divide Two Integers
tags:
  - LeetCode
mathjax: true
abbrlink: 61804
date: 2021-03-04 00:37:52
---

## Problem

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division, and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero, which means losing its fractional part. For example, `truncate(8.345) = 8` and `truncate(-2.7335) = -2`.

**Note:**

- Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For this problem, assume that your function **returns 231 − 1 when the division result overflows**.

<!-- more -->

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = truncate(3.33333..) = 3.
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = truncate(-2.33333..) = -2.
```

**Example 3:**

```
Input: dividend = 0, divisor = 1
Output: 0
```

**Example 4:**

```
Input: dividend = 1, divisor = 1
Output: 1
```

**Constraints:**

- `-231 <= dividend, divisor <= 231 - 1`
- `divisor != 0`

------

## Analysis

&emsp;&emsp;题目给出一个被除数（dividend）和除数（divisor），要求在不使用乘法、除法和模运算的前提下，完成除法运算。既然不能直接除，那就只能一个一个减了。以Example1为例，我们是怎么计算呢？用10减去3得到7，答案自增1；再用7减去3得到4，答案自增1；再用4减去3得到1，答案自增1，此时答案就是3了。

&emsp;&emsp;我们按照这个思路，来看看能不能把这个算法进一步进行优化，换一个被除数40，除数还是3，答案是13，所以说按照上面的步骤，我们一共有13次循环，每次答案自增1，最后才能得到答案13。我们优化的思路就是看怎么样不一步一步算到这个13，而是采用更加快速的方法。先来用3分解40，`40 = 8 * 3 + 4 * 3 + 1 * 3 + 1 `，可以看到3前面的系数是8、4、1，是2的次方。而实际上前面的系数就是把答案13的二进制进行了拆解。所以这里我们就可以找到规律了：

1. 系数是2的n次方，先找到最大的系数$2 ^ n$，使得`2^n * 3 `不大于40，此时找到了`n = 3`，所以系数是8，答案加上这个系数；此时被除数变成了`40 - 8 * 3 = 16`；
2. 然后再找到最大的系数$2^n$，使得`2^n * 3`不大于16，此时找到了`n = 2`，所以系数是4，答案加上这个系数为12；此时被除数变成了`16 - 4 * 3 = 4`；
3. 然后找到最大的系数$2^n$，使得`2^n * 3`不大于4，此时找到了`n = 1`，试试所以系数是1，答案加上这个系数为13；此时被除数变成了`4 - 1 * 3 = 1`，这个时候1小于除数3，计算结束。

------

## Solution

&emsp;&emsp;因为找出最大系数的时候是2的次方，而题目不允许使用乘法，所以这里用位运算代替。同时要注意正负号的处理，因为正负数并不影响结果，所以可以先记录下正负号，然后都转换为正数的运算，最终返回结果的时候添加正负。

------

## Code

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN && divisor == -1) return INT_MAX;
        long m = labs(dividend), n = labs(divisor), res = 0;
        int sign = ((dividend < 0) ^ (divisor < 0)) ? -1 : 1;
        if (n == 1) return sign == 1 ? m : -m;
        while (m >= n) {
            long t = n, p = 1;
            while (m >= (t << 1)) {
                t <<= 1;
                p <<= 1;
            }
            res += p;
            m -= t;
        }
        return sign == 1 ? res : -res;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道数学类题目，本质上还是运用到了位运算。解决数学的加减乘除类型的题目要多考虑二进制和位运算，这些都是能够加快运算速度的方法。这道题目的分享到这里，感谢你的支持！
