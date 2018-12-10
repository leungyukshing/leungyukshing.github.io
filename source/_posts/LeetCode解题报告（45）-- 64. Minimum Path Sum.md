---
title: LeetCode解题报告（45）-- 64. Minimum Path Sum
tags:
  - LeetCode
mathjax: true
abbrlink: 16593
date: 2018-12-10 12:25:44
---
## Problem
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.
<!-- more -->

**Example:**
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```
## Analysis
&emsp;&emsp;这道题也是动态规划的题目，比起上一题，这题的难度要稍微增加，状态转移从一维变成了二维，但基本的思路是一样的。对于每一个格子来说，**只能从它的左边或上边过来**，这样我们就可以写出动态转移的方程了。
&emsp;&emsp;我们需要对`cost`矩阵进行初始化，最左边和最上边是要手动初始化的（特别地，`cost[0][0] = 0`）。最左边的每一个元素都是它上面元素cost之和，最上边的每一个元素都是它左边元素cost之和。其余的从上到下、从左到右地遍历就可以了。

&emsp;&emsp;状态转移方程：
$$
cost(i,j) = \begin{cases}
grid(i-1,0)+cost(i-1,0),  & \text{if $j=0$ and $i\geq1$} \\
grid(0,j-1)+cost(0,j-1),  & \text{if $i=0$ or $j\geq1$} \\
min(cost(i-1,j)+grid(i-1,j),cost(i,j-1)+grid(i,j-1)),  & \text{otherwise}
\end{cases}
$$

## Solution
&emsp;&emsp;新建一个同`grid`大小一样的矩阵`cost`，初始化第一行和第一列，然后按照状态转移方程进行遍历，最后返回`cost[m-1][n-1] + grid[m-1][n-1]`即可。

## Code
```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int cost[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                cost[i][j] = 0;
            }
        }

        for (int i = 1; i < m; i++) {
            cost[i][0] = grid[i-1][0] + cost[i-1][0];
        }
        for(int j = 1; j < n; j++) {
            cost[0][j] = grid[0][j-1] + cost[0][j-1];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                cost[i][j] = min(cost[i-1][j] + grid[i-1][j], cost[i][j-1]+grid[i][j-1]);
            }
        }
        return cost[m-1][n-1]+grid[m-1][n-1];
    }
};
```
**运行时间：**约4ms，超过100%的CPP代码。
## Summary
&emsp;&emsp;这道题也是动态规划的题目的进阶版，比起上一题难度有所增加，但是只要把握住状态转移的规律，确定好状态转移的方向，实现起来也非常容易。这道题的分析就到这里，谢谢您的支持！
