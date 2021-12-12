---
title: LeetCode解题报告（412) -- 790. Domino and Tromino Tiling
mathjax: true
date: 2021-12-11 20:45:52
---

## Problem

You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

<!-- more -->

![img](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return *the number of ways to tile an* `2 x n` *board*. Since the answer may be very large, return it **modulo** `109 + 7`.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

**Example 1:**

![example1](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg))

```
Input: n = 3
Output: 5
Explanation: The five different ways are show above.
```

**Example 2:**

```
Input: n = 1
Output: 1
```

**Constraints:**

- `1 <= n <= 1000`

------

## Analysis

&emsp;&emsp;题目给出了两种方块，一种是domino，两个拼一起；另一种是tromino，是像L型的。然后题目给出一个`n`，问在`2 * n`的矩阵中，最多有多少种不同的拼凑方法。**最重要的是**，题目说答案会很大，要取模。一般来说这种就是很明显的dp提示。既然知道是dp了，那我们就先来看看状态和转移方程。

&emsp;&emsp;这道题目状态也非常简单，就只有一个`n`，所以状态也没什么分析的地方，可以把精力集中在转移方程。我们先把前几种情况列举一下。

+ 当`n = 0`时，虽然并没有摆放的可能，但是也初始化为1，为了统一后面的计算过程；
+ 当`n = 1`时，只有一种答案，就是domino竖着放；
+ 当`n = 2`时，有两种答案，两个domino竖着放或者横着放；
+ 当`n = 3`时，有5种答案，在`n = 1`的基础上，两个domino横着放，在`n = 2`的基础上，后面加上一个domino竖着放，在`n = 0`的基础上，两个tromino交错放。

&emsp;&emsp;当向后列举多几个之后就会发现，`dp[i] = dp[i - 1] + dp[i - 2] + 2 * (dp[i - 3] + ... + dp[0])`。然后我们进行化简，从后面拆一个`dp[i - 3]`出来，`dp[i] = dp[i - 1] + dp[i - 3] + dp[i - 2] + dp[i - 3] + 2 * (dp[i - 4] + .. + dp[0])`，后面部分其实就是`dp[i - 1]`，所以综合后得到：`dp[i] = 2 * dp[i - 1] + dp[i - 3]`。

&emsp;&emsp;至此，转移方程我们也有了，根据这个方程计算即可。

## Solution

&emsp;&emsp;需要注意每次循环运算后都取模。

------

## Code

```c++
class Solution {
public:
    int numTilings(int n) {
        if (n <= 2) {
            return n;
        }
        int M = 1e9 + 7;
        vector<long> dp(n + 1, 0);
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) {
            dp[i] = (2 * dp[i - 1] + dp[i - 3]) % M;
        }
        return dp[n];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是提示性非常明显的dp，但是找dp的过程比较难。说白了就是列举4-5种情况然后找规律，同时还需要对dp方程进行一点的变换化简。这道题目的分享到这里，感谢你的支持！

