---
title: LeetCode解题报告（497）-- 1765. Map of Highest Peak
mathjax: true
date: 2022-01-04 22:59:15
---

## Problem

You are given an integer matrix `isWater` of size `m x n` that represents a map of **land** and **water** cells.

- If `isWater[i][j] == 0`, cell `(i, j)` is a **land** cell.
- If `isWater[i][j] == 1`, cell `(i, j)` is a **water** cell.

You must assign each cell a height in a way that follows these rules:

- The height of each cell must be non-negative.
- If the cell is a **water** cell, its height must be `0`.
- Any two adjacent cells must have an absolute height difference of **at most** `1`. A cell is adjacent to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

Find an assignment of heights such that the maximum height in the matrix is **maximized**.

Return *an integer matrix* `height` *of size* `m x n` *where* `height[i][j]` *is cell* `(i, j)`*'s height. If there are multiple solutions, return **any** of them*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82045-am.png)

```
Input: isWater = [[0,1],[0,0]]
Output: [[1,0],[2,1]]
Explanation: The image shows the assigned heights of each cell.
The blue cell is the water cell, and the green cells are the land cells.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82050-am.png)

```
Input: isWater = [[0,0,1],[1,0,0],[0,0,0]]
Output: [[1,1,0],[0,1,1],[1,2,2]]
Explanation: A height of 2 is the maximum possible height of any assignment.
Any height assignment that has a maximum height of 2 while still meeting the rules will also be accepted.
```



**Constraints:**

- `m == isWater.length`
- `n == isWater[i].length`
- `1 <= m, n <= 1000`
- `isWater[i][j]` is `0` or `1`.
- There is at least **one** water cell.

---

## Analysis

&emsp;&emsp;这道题目给出一个矩阵，里面0表示陆地，1表示水。水的高度一定是0，而相邻两个位置的高度差最多为1，要使得整个矩阵最高的高度是最高的，要求返回最后的矩阵。这一眼就能看出是BFS了，因为相邻的高度差为1，所以只要是相邻我就把高度提高1，这样算出来的高度才是最高的，我们以所有水的位置作为BFS起点，然后维护一个`visited`矩阵防止重复遍历，最后把整个矩阵返回即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) {
        int m = isWater.size(), n = isWater[0].size();
        queue<pair<int, int>> q;
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (isWater[i][j] == 1) {
                    isWater[i][j] = 0;
                    q.push({i, j});
                    visited[i][j] = true;
                } else {
                    isWater[i][j] = INT_MAX;
                }
            }
        }
        
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};
        int dis = 0;
        while (!q.empty()) {
            int size = q.size();
            //cout << "level: " << dis << endl;
            for (int i = 0; i < size; ++i) {
                auto cur = q.front();
                q.pop();
                int x = cur.first;
                int y = cur.second;

                //cout << "handle " << x << " " << y << endl;
                isWater[x][y] = dis;
                
                for (int k = 0; k < 4; ++k) {
                    int newX = x + dx[k];
                    int newY = y + dy[k];
                    
                    if (newX < 0 || newX >= m || newY < 0 || newY >= n || isWater[newX][newY] != INT_MAX || visited[newX][newY]) {
                        continue;
                    }
                    q.push({newX, newY});
                    visited[newX][newY] = true;
                }
            }
            ++dis;
        }
        return isWater;
        
    }
};
```

------

## Summary

&emsp;&emsp;这道题目其实就是经典的BFS加上高度信息。这道题目的分享到这里，感谢你的支持！

