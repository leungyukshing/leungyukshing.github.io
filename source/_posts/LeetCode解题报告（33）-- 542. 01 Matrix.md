---
title: LeetCode解题报告（33）-- 542. 01 Matrix
tags:
  - LeetCode
abbrlink: 38012
date: 2018-11-12 13:00:30
---
## Problem
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.
<!-- more -->

**Example 1**:
```
Input:
0 0 0
0 1 0
0 0 0

Output:
0 0 0
0 1 0
0 0 0
```

**Example 2**:
```
Input:
0 0 0
0 1 0
1 1 1

Output:
0 0 0
0 1 0
1 2 1
```

**Note:**
  + The number of elements of the given matrix will not exceed 10,000.
  + There are at least one 0 in the given matrix.
  + The cells are adjacent in only four directions: up, down, left and right.

## Analysis
&emsp;&emsp;这道题求的是每一个点到0的最短路径，第一反应有点像是最短路问题，但是这里给出的是矩阵，性质非常好，因此想到使用类似于走迷宫的BFS算法来实现。我们将0视为是起点，将非0的位置视为是终点。这样我们就可以遍历整幅图了，但是如何保证是最短路径呢？
&emsp;&emsp;这里用到了贪心算法的思想，即当我要计算下一个点的路径时，与当前点的路径做比较，如果**当前值>下一个值**，则说明有另外一条更短的路径，因此就不需要操作了；否则，则说明**下一个点可由当前点最短达到**！

## Solution
&emsp;&emsp;使用queue进行BFS，注意queue存放的是一个坐标，将为0的点都放入queue中，然后进行BFS。这里为了利用贪心算法的思想，必须将其余非0的点先初始化为INT_MAX（即无穷），以保证获得的路径是最短的，而这不会改变结果。因为起点是固定的，而我们只会拓展比当前点值大的点，因此所有点都是可以被访问到的。

## Code
```C++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        queue<pair<int, int>> q;
        int row = matrix.size();
        int column = matrix[0].size();
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (matrix[i][j] == 0) {
                    q.push(make_pair(i, j));
                }
                else {
                    matrix[i][j] = INT_MAX;
                }
            }
        }

        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};

        while (!q.empty()) {
            pair<int, int> p = q.front();
            q.pop();
            for (int i = 0; i < 4; i++) {
                int newX = p.first+dx[i];
                int newY = p.second+dy[i];
                if (newX >= 0 && newX < row && newY >= 0 && newY < column && matrix[newX][newY] > matrix[p.first][p.second]) {
                    matrix[newX][newY] = matrix[p.first][p.second]+1;
                    q.push(make_pair(newX, newY));
                }
            }
        }
        return matrix;
    }
};
```
**运行时间：**约144ms，超过86.53%的CPP代码。

## Summary
&emsp;&emsp;这道题一开始没想到使用贪心算法，而是每个点进行遍历，直到0，这样的做法效率是很低的。后来考虑到了使用BFS，结合贪心算法，实现起来就快很多了。巧妙之处是要将非0的点初始化一个INT_MAX，这是实现贪心算法的关键！这道题的分析到这里，感谢您的支持！
