---
title: LeetCode解题报告（98）-- 463. Island Perimeter
tags:
  - LeetCode
mathjax: true
abbrlink: 15737
date: 2020-07-08 00:49:10
---

## Problem

You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

<!-- more -->

**Example:**

```
Input:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

Output: 16
```

**Explanation**: The perimeter is the 16 yellow stripes in the image below:

![example image](https://assets.leetcode.com/uploads/2018/10/12/island.png)

------

## Analysis

&emsp;&emsp;这道题目关于二维数组的，主要就是求岛屿的周长。由于题目规定了只有一个岛屿，所以这道题目的难度降低了不少。对于值为1的位置，我们只需要判断他的四周是否水（值为0）就可以了。

------

## Solution

&emsp;&emsp;判断的时候需要考虑四条边的边界情况。

------

## Code

```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;
        int m = grid.size(), n = grid[0].size(), res = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) continue;
                if (j == 0 || grid[i][j - 1] == 0) ++res;
                if (i == 0 || grid[i - 1][j] == 0) ++res;
                if (j == n - 1 || grid[i][j + 1] == 0) ++res;
                if (i == m - 1 || grid[i + 1][j] == 0) ++res;
            }
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，重要的是了解到二维数组中类似的求周长、面积的思路。同时还需要注意边界情况。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
