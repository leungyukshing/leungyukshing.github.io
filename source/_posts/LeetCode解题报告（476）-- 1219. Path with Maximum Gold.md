---
title: LeetCode解题报告（476）-- 1219. Path with Maximum Gold
mathjax: true
date: 2021-12-29 17:05:47
---

## Problem

In a gold mine `grid` of size `m x n`, each cell in this mine has an integer representing the amount of gold in that cell, `0` if it is empty.

Return the maximum amount of gold you can collect under the conditions:

- Every time you are located in a cell you will collect all the gold in that cell.
- From your position, you can walk one step to the left, right, up, or down.
- You can't visit the same cell more than once.
- Never visit a cell with `0` gold.
- You can start and stop collecting gold from **any** position in the grid that has some gold.

<!-- more -->

**Example 1:**

```
Input: grid = [[0,6,0],[5,8,7],[0,9,0]]
Output: 24
Explanation:
[[0,6,0],
 [5,8,7],
 [0,9,0]]
Path to get the maximum gold, 9 -> 8 -> 7.
```

**Example 2:**

```
Input: grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
Output: 28
Explanation:
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
Path to get the maximum gold, 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7.
```



**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 15`
- `0 <= grid[i][j] <= 100`
- There are at most **25** cells containing gold.

---

## Analysis

&emsp;&emsp;这道题目就是一个简单的DFS遍历，每个位置开始遍历一次，统计出总和即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int getMaximumGold(vector<vector<int>>& grid) {
        m = grid.size(), n = grid[0].size();
        int result = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    //cout << "start at " << i << " " << j << endl;
                    result = max(result, dfs(grid, i, j));                    
                }
            }
        }
        return result;
    }
private:
    int m, n;
    int dfs(vector<vector<int>>& grid, int i, int j) {
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};
        
        int temp = grid[i][j];
        grid[i][j] = 0; // mark as visited
        int total = 0;
        //cout << "handle " << i << " " << j << endl;
        for (int k = 0; k < 4; ++k) {
            int newI = i + dx[k];
            int newJ = j + dy[k];
            if (newI < 0 || newI >= m || newJ < 0 || newJ >= n || grid[newI][newJ] == 0) {
                continue;
            }
            total = max(total, dfs(grid, newI, newJ));
        }
        
        grid[i][j] = temp;
        return total + temp;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较简单的二维矩阵DFS。这道题目的分享到这里，感谢你的支持！

