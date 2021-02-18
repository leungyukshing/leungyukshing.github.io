---
title: LeetCode解题报告（297)-- 413. Arithmetic Slices
tags:
  - LeetCode
mathjax: true
date: 2021-02-18 21:47:42

---

## Problem

A sequence of numbers is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

<!-- more -->

For example, these are arithmetic sequences:

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

The following sequence is not arithmetic.

```
1, 1, 2, 5, 7
```

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of the array A is called arithmetic if the sequence:
A[P], A[P + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.

**Example:**

```
A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求我们计算出其中有多少个**等差数列切片**。其中等差数列切片的定义是长度至少为3的等差数列。遇上等差数列，而且求的是数量，优先考虑DP。我们定义`dp[i]`是以`i`结尾的等差数列中有多少个数列切片。状态转移非常简单，只需要在前一个状态的基础上+1即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& A) {
        int size = A.size();
        vector<int> dp(size, 0);
        int result = 0;
        
        for (int i = 2; i < size; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp[i] = dp[i - 1] + 1;
                result += dp[i];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是DP的一道简单应用，在等差数列类型的题目中dp是非常常用的解法。这道题目的分享到这里，谢谢您的支持！
