---
title: LeetCode解题报告（359) -- 63. Unique Paths II
tags:
  - LeetCode
mathjax: true
abbrlink: 21128
date: 2021-05-02 12:13:06
---

## Problem

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

**Constraints:**

- `m == obstacleGrid.length`
- `n == obstacleGrid[i].length`
- `1 <= m, n <= 100`
- `obstacleGrid[i][j]` is `0` or `1`.

------

## Analysis

&emsp;&emsp;比较常规的二维矩阵移动问题，题目要求我们计算的是从左上角出发，到达右下角有多少条不一样的路径。其中矩阵中会有一些不可以行走的点。同时规定了移动的要求是每个格子只能往右边或下边移动，这样看来就是很简单的二维dp了。

&emsp;&emsp;首先初始化第一行和第一列，然后从上到下，从左往右遍历即可，这个遍历的顺序和移动的规则要求是保持一致的。如果遇到障碍，则把改点的dp值改为0；否则，则把该点上方和左方的值相加。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        if (obstacleGrid[0][0] == 1) {
            return 0;
        }
        dp[0][0] = 1;
        for (int i = 1; i < m; i++) {
            if (obstacleGrid[i][0] != 1) {
                dp[i][0] = dp[i - 1][0];
            }
        }
        for (int i = 1; i < n; i++) {
            if (obstacleGrid[0][i] != 1) {
                dp[0][i] = dp[0][i - 1];
            }
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道题是很常规的二维dp，刷过很多类似题目的话，这种题目就是秒杀。这道题目的分享到这里，感谢你的支持！
