---
title: LeetCode解题报告（32）-- 200. Number of Islands
tags:
  - LeetCode
abbrlink: 12528
date: 2018-11-12 12:47:52
---
## Problem
Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
<!-- more -->

**Example 1:**
```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**
```
Input:
11000
11000
00100
00011

Output: 3
```

## Analysis
&emsp;&emsp;这道问题表面上很复杂，其实简化后实质是一个求连通分量的题目。每一个Island就是一个连通分量。连通的条件是相邻都是1（水平方向或垂直方向，斜线不算）。把握住问题的实质后，我们可以使用DFS的方法来求解这个问题。需要另外增多一个矩阵来表明某个点是否被访问过，然后需要对每一个点相邻的点进行判断，如果是1则继续遍历，直至到边界或没有相邻的1。

## Solution
&emsp;&emsp;使用一个`isChecked`的bool类型二维数组来表面每个点是否被访问过。然后在主函数中调用dfs，每次调用岛屿数量加一。特别注意访问后需要将该点的isChecked设为true。

## Code
```C++
int dx[] = {-1, 0, 0 ,1};
int dy[] = {0, -1, 1, 0};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if (grid.size() == 0) {
            return 0;
        }
        int islandNum = 0;
        row = grid.size();
        column = grid[0].size();
        vector<vector<bool>> isChecked(row, vector<bool>(column, false));
        /*
        for (int i = 0 ; i < row; i++) {
            for (int j = 0; j < column; j++) {
                isChecked[i][j] = false;
            }
        }
        */
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (grid[i][j] == '1' && !isChecked[i][j]) {
                    islandNum++;
                    isChecked[i][j] = true;
                    dfs(i, j, grid, isChecked);
                }
            }
        }
        return islandNum;
    }
private:
    int row;
    int column;
    void dfs(int x, int y, vector<vector<char>>& grid, vector<vector<bool>>& isChecked) {
        for (int i = 0; i < 4; i++) {
            int newX = x + dx[i];
            int newY = y + dy[i];
            if (newX >= 0 && newX < row && newY >= 0 && newY < column && grid[newX][newY] == '1' && !isChecked[newX][newY]) {
                //cout << newX << " " << newY << endl;
                isChecked[newX][newY] = true;
                dfs(newX, newY, grid, isChecked);
            }
        }
    }
};
```
**运行时间：**约8ms，超过98.93%的CPP代码。

## Summary
&emsp;&emsp;这道题表面很复杂，但是如果能看懂实质是求连通分量的数量后，问题就变得很简单了。常规方法就是使用DFS来求解，需要特别注意这里的连通条件是相邻，需要自己用两个数组来实现某个点向四周的拓展过程，注意越界问题即可，其他没什么难度。本题的分析到此为止，谢谢您的支持！
