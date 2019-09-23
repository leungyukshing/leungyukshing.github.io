---
title: LeetCode解题报告（57）-- 62. Unique Paths
tags:
  - LeetCode
date: 2019-09-23 20:07:19


---

## Problem

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

<!-- more -->

![robot_maze](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

**Example 2:**

```
Input: m = 7, n = 3
Output: 28
```

------

## Analysis

&emsp;&emsp;这道题是比较典型的二维形式的动态规划问题。根据题意，机器人只能往右或往下走，所以运动的方式是有规律的，每一个方块只能从左边或上边到来，所以我们只需要把两个方向的路径数相加即可。然后我们还需要单独处理边界情况，如第一行、第一列和初始化第一个元素。

------

## Solution

&emsp;&emsp;初始化$f[0][0] = 1$，其余的推导公式为：
$$
f[i][j] = \left\{\begin{aligned}
f[i][j - 1] &, i = 0 \\\\
f[i - 1][j] &, j = 0 \\\\
f[i][j - 1] + f[i - 1][j] &, otherwise
\end{aligned}
\right.
$$

------

## Code

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        int f[m][n];
        f[0][0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    continue;
                }
                if (i == 0) {
                    f[i][j] = f[i][j - 1];
                }
                else if (j == 0) {
                    f[i][j] = f[i - 1][j];
                }
                else {
                    f[i][j] = f[i][j - 1] + f[i - 1][j];
                }
            }
        }
        return f[m - 1][n - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这是一道典型的动态规划题目，是比较简单的二维形式的动态规划。做这类题目的时候要把握好动态规划的思路：每一步的计算都依赖于上一步或上两步的结果。然后确定初始状态以及边界条件即可。这道题的分享就到这里，欢迎大家评论、转发，感谢您的支持！

