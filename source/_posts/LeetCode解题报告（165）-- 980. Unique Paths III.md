---
title: LeetCode解题报告（165）-- 980. Unique Paths III
tags:
  - LeetCode
mathjax: true
date: 2020-09-21 14:55:02

---

## Problem

On a 2-dimensional `grid`, there are 4 types of squares:

- `1` represents the starting square.  There is exactly one starting square.
- `2` represents the ending square.  There is exactly one ending square.
- `0` represents empty squares we can walk over.
- `-1` represents obstacles that we cannot walk over.

Return the number of 4-directional walks from the starting square to the ending square, that **walk over every non-obstacle square exactly once**.

<!-- more -->

**Example 1:**

```
Input: [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
Output: 2
Explanation: We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```

**Example 2:**

```
Input: [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
Output: 4
Explanation: We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```

**Example 3:**

```
Input: [[0,1],[2,0]]
Output: 0
Explanation: 
There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.
```

**Note:**

1. `1 <= grid.length * grid[0].length <= 20`

------

## Analysis

&emsp;&emsp;这道题是非常经典的地图回溯题目。这类题目主要就是在方格中往四个方向行走，求一些路径的长度或者路径的数量。那么对付这类题目很好的方法就是回溯，回溯的意思就是在某个位置的时候，利用递归向各个方向都走，走完之后又回退到当前的位置。

&emsp;&emsp;结合这道题目分析，题目要求的是计算出**走完所有空格的路径的数量**。走完所有空格这个是比较好判断的，只需要记录下走过的空格数量，如果和给出的空格数量一致，那么就是走完了。终止条件已经知道了，接下来就来看怎么走。

&emsp;&emsp;首先可以往四个方向走，这个不难，走完某个空格之后，需要先标志为-1（不可走），然后往四个方向都试完之后，再重新标记回原来的值。

------

## Solution

&emsp;&emsp;递归的时候带上parent的坐标还有空格数量的统计信息即可。

------

## Code

```c++
class Solution {
public:
    int uniquePathsIII(vector<vector<int>>& grid) {
        int result = 0;
        int totalCount = 0;
        
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    totalCount++;
                }
            }
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    dfs(grid, i, j, 0, totalCount, result);
                }
            }
        }
        return result;
    }
private:
    void dfs(vector<vector<int>>& grid, int parent_x, int parent_y, int currentCount, int totalCount, int& result) {
        if (grid[parent_x][parent_y] == 2 && currentCount == totalCount) {
            result++;
            return;
        }
        int m = grid.size();
        int n = grid[0].size();
        int parent = grid[parent_x][parent_y];
        if (parent == 0) {
            currentCount++;
        }
        grid[parent_x][parent_y] = -1;
        for (auto d : dirs) {
            int next_x = parent_x + d.first;
            int next_y = parent_y + d.second;
            if (next_x < 0 || next_x >= m || next_y < 0 || next_y >= n || grid[next_x][next_y] == -1)
                continue;
            dfs(grid, next_x, next_y, currentCount, totalCount, result);
        }
        grid[parent_x][parent_y] = parent;
    }
    
    vector<pair<int, int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
};
```

------

## Summary

&emsp;&emsp;这道题目被标记为hard，虽然代码量比较多，但是如果是熟悉回溯的朋友应该能够快速解决。这道题的分析到这里，谢谢！
