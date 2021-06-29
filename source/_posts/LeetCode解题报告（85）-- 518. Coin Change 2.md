---
title: LeetCode解题报告（85）-- 518. Coin Change 2
tags:
  - LeetCode
mathjax: true
abbrlink: 27633
date: 2020-06-09 11:47:52
---

## Problem

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

<!-- more -->

**Example 1:**

```
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```
Input: amount = 10, coins = [10] 
Output: 1
```

**Note:**

You can assume that

- 0 <= amount <= 5000
- 1 <= coin <= 5000
- the number of coins is less than 500
- the answer is guaranteed to fit into signed 32-bit integer

------

## Analysis

&emsp;&emsp;首先这道题目要计算的是方案的数量，我们面临着两个问题：

1. 选不选
2. 选多少个

&emsp;&emsp;看到这种问题的套路，很自然地就会往dp的方向考虑了。因为每一个选择，都会基于上一步的选择。先来看看动态转换方程`d[i][j]`，它表示的是前`i`个硬币总额为`j`的时候方案的数量。这个二维数组的计算方式是从左到右，从上到下。从左到右是金额增加的方向，从上到下是硬币数量增加的方向。所以它的计算公式为：
$$
d[i][j] = d[i - 1][j] + d[i][j - coins[i]]
$$
&emsp;&emsp; 第一项指的是当前这个硬币不选，所以总额不变，和上方的方案数是一样的；第二项指的是选择当前这个硬币，因为选择了当前这个硬币，所以总额要减去当前这个硬币的面值，才找到减去面值后的方案数。换个角度理解，`i`控制的是不同面值的硬币，而`j`控制的是同一种面值下硬币的数量。

&emsp;&emsp;使用上面这种二维数组实际上已经可以解决问题，但是我们还可以进一步优化，原因是它的计算方向是固定的。即每一个格子，都是由上边或左边的结果得到，所以只要我们按照顺序来计算，可以使用一维数组替代。我们把`i`这一维度压缩，所以得到新的状态转换方程：
$$
d[j] = d[j] + d[j - coins[i]]
$$

------

## Solution

&emsp;&emsp;在上面的公式中，需要判断`j - coins[i]`大于0，实际上可以把`j`初始化为`coins[i]`，这样就避免了不停的判断。

------

## Code

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> d(amount + 1, 0);
        d[0] = 1;
        
        for (int i = 0; i < coins.size(); i++) {
            for (int j = coins[i]; j <= amount; j++) {
                d[j] += d[j - coins[i]];
            }
        }
        return d[amount];
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目还算是比较经典的dp问题，但是通过一步一步的推导，发现还是有优化的空间的，也能更好地理解dp的思路。希望这篇博客能够帮助到大家，谢谢！
