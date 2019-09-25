---
title: LeetCode解题报告（57）-- 62. Unique Paths
tags:
  - LeetCode
date: 2019-09-25 13:10:49



---

## Problem


A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

<!-- more -->

![robot_maze](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

An obstacle and empty space is marked as `1` and `0`respectively in the grid.

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

------

## Analysis

&emsp;&emsp;这道题是[62. Unique Paths]([http://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%8857%EF%BC%89--%2062.%20Unique%20Paths.html](http://leungyukshing.cn/archives/LeetCode解题报告（57）-- 62. Unique Paths.html))的改进版。在第一题的基础上，这道题添加了新的限制，如果某个空格是1，则说明该空格不能走。因为robot运动的规则和第一题是相同的，所以我们只需要针对这个限制额外加条件就可以了。如果某个空格是1，则说明该空格的下方和右方都不能从这个格子得到。

------

## Solution

&emsp;&emsp;初始化$f[0][0] = 1$，其余的推导公式为：
$$
f[i][j] = \left\{\begin{aligned}f[i][j - 1] &, i = 0 \\\\f[i - 1][j] &, j = 0 \\\\f[i][j - 1] + f[i - 1][j] &, otherwise\end{aligned}\right.
$$
然后添加限制条件，我们可以直接设置$f[i][j] = 0$，意思是没有办法到该空格。这个和不能从该空格出发到其他空格是等价的，因为我们只要从起点到终点的路径，中间的过程只要一个空格断了，该路径就可以不用考虑。

------

## Code

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        if (m == 0) {
            return 0;
        }
        int n = obstacleGrid[0].size();
        if (n == 0) {
            return 0;
        }
        
        unsigned int f[m][n];
        f[0][0] = !obstacleGrid[0][0];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    continue;
                }
                if (obstacleGrid[i][j] == 1) {
                    f[i][j] = 0;
                }
                else {
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
        }
        return f[m - 1][n - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道动态规划题目稍加了一点限制，难度不算很大，如果能够很好地把握第一题，那么这道题的难点就在于添加限制。这道题的分享到这里，欢迎大家转发、评论，感谢您的支持！
