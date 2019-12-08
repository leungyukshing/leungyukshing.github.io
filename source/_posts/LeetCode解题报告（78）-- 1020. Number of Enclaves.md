---
title: LeetCode解题报告（78）-- 1020. Number of Enclaves
tags:
  - LeetCode
date: 2019-12-08 23:29:10
mathjax: true
---

## Problem

Given a 2D array `A`, each cell is 0 (representing sea) or 1 (representing land)

A move consists of walking from one land square 4-directionally to another land square, or off the boundary of the grid.

Return the number of land squares in the grid for which we **cannot** walk off the boundary of the grid in any number of moves.

<!-- more -->

**Example 1:**

```
Input: [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3
Explanation: 
There are three 1s that are enclosed by 0s, and one 1 that isn't enclosed because its on the boundary.
```

**Example 2:**

```
Input: [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
Output: 0
Explanation: 
All 1s are either on the boundary or can reach the boundary.
```

**Note:**

1. `1 <= A.length <= 500`
2. `1 <= A[i].length <= 500`
3. `0 <= A[i][j] <= 1`
4. All rows have the same size.

------

## Analysis

&emsp;&emsp;题目给出一个方格图，0表示海洋，1表示陆地。我们只能从一个陆地向**上、下、左、右**四个方向移动到下一个陆地，从边界出发，我们需要求出无法到达的陆地的数目。

&emsp;&emsp;二维平面上的连通性问题可以通过BFS或DFS来求解。我们使用一个除了0和1的数字来标记**可连通的范围**。我们从边界的位置开始，首先将陆地标记为可连通。然后采用BFS，对于每一个连通的位置，再向它的四周搜索，如果四周的位置仍然是陆地，则继续标记为可连通。最终，再次遍历整个图，找出没有被修改为可连通状态的陆地的数量即可。

------

## Solution

&emsp;&emsp;因为题目要求是从边界出发，所以我们要首先处理四条边界的情况。我们使用-1标记可连通的格子，然后采用BFS对整个方格图进行遍历，最后找出图中值为1的格子数量即可。

------

## Code

```c++
class Solution {
public:
    int numEnclaves(vector<vector<int>>& A) {
        int m = A.size();
        int n = A[0].size();
        queue<pair<int, int>> q;
        
        int dx[] = {0, -1, 0, 1};
        int dy[] = {-1, 0, 1, 0};
        
        // check boundary
        for (int i = 0; i < m; i++) {
            if (A[i][0] == 1) {
                A[i][0] = -1;
            }
            if (A[i][n - 1] == 1) {
                A[i][n - 1] = -1;
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (A[0][i] == 1) {
                A[0][i] = -1;
            }
            if (A[m - 1][i] == 1) {
                A[m - 1][i] = -1;
            }
        }
        
        // BFS
        for (int i = 0 ; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (A[i][j] == -1) {
                    q.push(make_pair(i, j));
                    while (!q.empty()) {
                        pair<int, int> p = q.front();
                        q.pop();
                        for (int k = 0; k < 4; k++) {
                            int x = dx[k] + p.first;
                            int y = dy[k] + p.second;
                            if (x >= 0 && x < m && y >= 0 && y < n && A[x][y] == 1) {
                                A[x][y] = -1;
                                q.push(make_pair(x, y));
                            }
                        }
                    }
                }
            }
        }
        
        // count 1s in grid
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (A[i][j] == 1) {
                    result++;
                }
            }
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题是BFS应用的题目，处理二维平面的连通问题很可能需要标记状态，有时候新增一个状态比重新建一个副本来记录要简单的多。这道题的思路就和大家分享到这里，欢迎大家评论、转发，谢谢您的支持！