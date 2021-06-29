---
title: LeetCode解题报告（342) -- 329. Longest Increasing Path in a Matrix
tags:
  - LeetCode
mathjax: true
abbrlink: 27944
date: 2021-04-14 12:26:04
---

## Problem

Given an `m x n` integers `matrix`, return *the length of the longest increasing path in* `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

**Example 3:**

```
Input: matrix = [[1]]
Output: 1
```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `0 <= matrix[i][j] <= 231 - 1`

------

## Analysis

&emsp;&emsp;这是一道二维矩阵的遍历问题，一般都是先考虑DFS，但这道题目有点不一样。这道题目是没有指定起点和终点，同时他要找的是最长路径的长度。所以理论上说我们要把每个位置作为起点的情况都计算一遍，然后最后看看哪个的路径最长。

&emsp;&emsp; 大致的思路有了，每个位置都作为起点，然后用DFS递归下去，往四个方向走，如果没有比当前位置更大的元素，或者越界了就返回，递归函数就返回路径的长度。这样的解法是比较直观的，但是效率肯定很差。

&emsp;&emsp;以一维数组看，假如数组是`[1,2,3,4,5]`，第一次把1作为起点，第二次把2作为起点，在第二次计算的时候，实际上第一次已经都算过一遍了。这不就是dp吗？回到这道题目中，二维矩阵中的每个位置实际上都是一个状态，每个点都作为起点跑一次递归，实际上肯定有很多重复计算的，我们可以利用dp矩阵把这些计算结果存下来。所以`dp[i][j]`定义为以`[i,j]`为起点得到的最长路径。这样在递归的过程中，如果发现`dp[i][j]`不为0，说明前面的计算已经算过了，直接用就可以。

&emsp;&emsp;然后我们来看转移方程是什么。 我们只需要把四个方向递归的结果中，记录最大值，就是这个位置的dp值，然后全局对所有位置都跑一遍，也维护一个最大值，那么这个最大值就是答案了。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        if (matrix.empty() || matrix[0].empty()) {
            return 0;
        }
        m = matrix.size();
        n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                result = max(result, helper(matrix, dp, i, j));
            }
        }
        return result;
    }
private:
    int m, n;
    vector<vector<int>> dirs = {{0, -1}, {-1, 0}, {0, 1}, {1, 0}};
    int helper(vector<vector<int>>& matrix, vector<vector<int>>& dp, int i, int j) {
        if (dp[i][j]) {
            return dp[i][j];
        }
        
        int maxNum = 1;
        for (auto &dir: dirs) {
            int new_i = i + dir[0];
            int new_j = j + dir[1];
            if (new_i < 0 || new_i >= m || new_j < 0 || new_j >= n || matrix[i][j] >= matrix[new_i][new_j]) {
                continue;
            }
            int current_len = helper(matrix, dp, new_i, new_j) + 1;
            maxNum = max(maxNum, current_len);
        }
        dp[i][j] = maxNum;
        return maxNum;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二维矩阵DFS和dp的一个结合，涉及到了两个很重要的知识点。其本质还是DFS，只不过是在DFS上，存储了中间的计算结果，这也是dp的本质。这道题目的分享到这里，感谢你的支持！
