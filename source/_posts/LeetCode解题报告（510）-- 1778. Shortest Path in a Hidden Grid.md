---
title: LeetCode解题报告（510）-- 1778. Shortest Path in a Hidden Grid
mathjax: true
date: 2022-01-12 00:32:47
---

## Problem

This is an **interactive problem**.

There is a robot in a hidden grid, and you are trying to get it from its starting cell to the target cell in this grid. The grid is of size `m x n`, and each cell in the grid is either empty or blocked. It is **guaranteed** that the starting cell and the target cell are different, and neither of them is blocked.

You want to find the minimum distance to the target cell. However, you **do not know** the grid's dimensions, the starting cell, nor the target cell. You are only allowed to ask queries to the `GridMaster` object.

<!-- more -->

The `GridMaster` class has the following functions:

- `boolean canMove(char direction)` Returns `true` if the robot can move in that direction. Otherwise, it returns `false`.
- `void move(char direction)` Moves the robot in that direction. If this move would move the robot to a blocked cell or off the grid, the move will be **ignored**, and the robot will remain in the same position.
- `boolean isTarget()` Returns `true` if the robot is currently on the target cell. Otherwise, it returns `false`.

Note that `direction` in the above functions should be a character from `{'U','D','L','R'}`, representing the directions up, down, left, and right, respectively.

Return *the **minimum distance** between the robot's initial starting cell and the target cell. If there is no valid path between the cells, return* `-1`.

**Custom testing:**

The test input is read as a 2D matrix `grid` of size `m x n` where:

- `grid[i][j] == -1` indicates that the robot is in cell `(i, j)` (the starting cell).
- `grid[i][j] == 0` indicates that the cell `(i, j)` is blocked.
- `grid[i][j] == 1` indicates that the cell `(i, j)` is empty.
- `grid[i][j] == 2` indicates that the cell `(i, j)` is the target cell.

There is exactly one `-1` and `2` in `grid`. Remember that you will **not** have this information in your code.



**Example 1:**

```
Input: grid = [[1,2],[-1,0]]
Output: 2
Explanation: One possible interaction is described below:
The robot is initially standing on cell (1, 0), denoted by the -1.
- master.canMove('U') returns true.
- master.canMove('D') returns false.
- master.canMove('L') returns false.
- master.canMove('R') returns false.
- master.move('U') moves the robot to the cell (0, 0).
- master.isTarget() returns false.
- master.canMove('U') returns false.
- master.canMove('D') returns true.
- master.canMove('L') returns false.
- master.canMove('R') returns true.
- master.move('R') moves the robot to the cell (0, 1).
- master.isTarget() returns true. 
We now know that the target is the cell (0, 1), and the shortest path to the target cell is 2.
```

**Example 2:**

```
Input: grid = [[0,0,-1],[1,1,1],[2,0,0]]
Output: 4
Explanation: The minimum distance between the robot and the target cell is 4.
```

**Example 3:**

```
Input: grid = [[-1,0],[0,2]]
Output: -1
Explanation: There is no path from the robot to the target cell.
```



**Constraints:**

- `1 <= n, m <= 500`
- `m == grid.length`
- `n == grid[i].length`
- `grid[i][j]` is either `-1`, `0`, `1`, or `2`.
- There is **exactly one** `-1` in `grid`.
- There is **exactly one** `2` in `grid`.

---

## Analysis

&emsp;&emsp;和上面那题一样，这题也是有关hidden grid二题目。总体思路再提一遍：

1. 先用DFS把hidden grid构建出来，存储的方法有两种，`unordered_map`或者二维矩阵；
2. 转化为基本的图问题后，再利用BFS或者dijkstra等基本的图算法求解题目。

&emsp;&emsp;我们还是先看第二步，题目要求找最短路径，毫无疑问就是BFS了。然后我们来看第一步怎么做，上一道题目我用了`unordered_map`的方法存储grid，这道题目我就换一种方法，用二维矩阵来存储。这里我们假设起点是`(501, 501)`，那么总长度就应该是`1001`，这样才能保证不会越界。其他的就是基本的DFS把每个位置能否移动标记出来即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
/**
 * // This is the GridMaster's API interface.
 * // You should not implement it, or speculate about its implementation
 * class GridMaster {
 *   public:
 *     bool canMove(char direction);
 *     void move(char direction);
 *     boolean isTarget();
 * };
 */

class Solution {
public:
    int findShortestPath(GridMaster &master) {
        graph = vector<vector<int>>(1002, vector<int>(1002, 3));
        // targetI = INT_MAX, targetJ = INT_MAX;
        dfs(master, 501, 501);
        /*
        if (targetI == INT_MAX || targetJ == INT_MAX) {
            return -1;
        }
        */
        // cout << "finish dfs" << endl;
        
        // bfs
        // visited.clear();
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};
        queue<pair<int, int>> q;
        q.push({501, 501});
        
        int steps = 0;
        while (!q.empty()) {
            //cout << steps << endl;
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto cur = q.front();
                q.pop();
                
                int x = cur.first, y = cur.second;
                for (int k = 0; k < 4; ++k) {
                    int newI = x + dx[k];
                    int newJ = y + dy[k];
                    if (graph[newI][newJ] == 1) {
                        q.push({newI, newJ});
                        graph[newI][newJ] = 3;
                    } else if (graph[newI][newJ] == 2) {
                        // cout << "search target" << endl;
                        return steps + 1;
                    }
                }
            }
            ++steps;
        }
        return -1;
    }
private:
    vector<vector<int>> graph;
    // int targetI, targetJ;
    void dfs(GridMaster &master, int i, int j) {
        //cout << i << " " << j << endl;
        if (graph[i][j] != 3) {
            return;
        }
        graph[i][j] = 1;
        if (master.isTarget()) {
            //targetI = i;
            //targetJ = j;
            graph[i][j] = 2;
            return;
        }
        
        // int left = i * 501 + j - 1, right = i * 501 + j + 1, up = (i - 1) * 501 + j, down = (i + 1) * 501 + j;
        if (master.canMove('L') && graph[i][j - 1] == 3) {
            master.move('L');
            dfs(master, i, j - 1);
            master.move('R');
        }
        if (master.canMove('R') && graph[i][j + 1] == 3) {
            master.move('R');
            dfs(master, i, j + 1);
            master.move('L');
        }
        if (master.canMove('U') && graph[i - 1][j] == 3) {
            master.move('U');
            dfs(master, i - 1, j);
            master.move('D');
        }
        if (master.canMove('D') && graph[i + 1][j] == 3) {
            master.move('D');
            dfs(master, i + 1, j);
            master.move('U');
        }
    }
};
```

------

## Summary

&emsp;&emsp;连续两道有关hidden grid的题目，相信大家已经能把这种类型的题目弄清楚了。其本质还是先DFS构造出图，进而把题目转变成正常的图类型的题目来处理。这道题目的分享到这里，感谢你的支持！

