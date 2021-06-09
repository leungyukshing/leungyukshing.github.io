---
title: LeetCode解题报告（389) -- 695. Max Area of Island
tags:
  - LeetCode
mathjax: true
date: 2021-06-09 23:55:18

---

## Problem

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

**Example 2:**

```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
```



**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

------

## Analysis

&emsp;&emsp;这是一道二维矩阵遍历的题目，基本的思想还是DFS。但这次要计算的并不是从某个点到某个点的路径，而是面积，这是有不同的。计算路径时，我们只需要返回一个最大或最小的长度，但是计算面积时，需要把面积加起来。

&emsp;&emsp;每个格到四个方向移动的套路相信大家都很熟悉了，走过的地方也要标记一下，不能重复访问。然后每个格子把四个方向DFS的结果相加，最后加上1（自己当前这格所占的面积）。然后我们全局维护一个最大的面积即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        m = grid.size();
        n = grid[0].size();
        vector<vector<int>> visited(m, vector<int>(n, 0));
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0 || visited[i][j]) {
                    continue;
                }
                visited[i][j] = 1;
                // cout << "start" << i << ", " << j << endl;
                int size = dfs(grid, visited, i, j);
                result = max(result, size);
            }
        }
        return result;
    }
private:
    int dfs(vector<vector<int>>& grid, vector<vector<int>>& visited, int i, int j) {
        // cout << "start handle " << i << ", " << j << endl;
        int dx[] = {0, -1, 0, 1};
        int dy[] = {-1, 0, 1, 0};
        visited[i][j] = 1;
        int size = 1;
        for (int k = 0; k < 4; k++) {
            int new_i = i + dx[k];
            int new_j = j + dy[k];
            if (new_i < 0 || new_i >= m || new_j < 0 || new_j >= n || visited[new_i][new_j] || grid[new_i][new_j] == 0) {
                continue;
            }
            int temp = dfs(grid, visited, new_i, new_j);
            // cout << temp << endl;
            size += temp;
        }
        // cout << i << ", " << j << ": value is " << size << endl;
        return size;
    }
    
    int m,n;
};
```

------

## Summary

&emsp;&emsp;这道题虽然意思上稍作修改，从路径变成了面积，但是整体的算法思路是一样的。这道题目的分享到这里，感谢你的支持！
