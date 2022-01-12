---
title: LeetCode解题报告（509）-- 1810. Minimum Path Cost in a Hidden Grid
mathjax: true
date: 2022-01-10 22:55:12
---

## Problem

This is an **interactive problem**.

There is a robot in a hidden grid, and you are trying to get it from its starting cell to the target cell in this grid. The grid is of size `m x n`, and each cell in the grid is either empty or blocked. It is **guaranteed** that the starting cell and the target cell are different, and neither of them is blocked.

Each cell has a **cost** that you need to pay each time you **move** to the cell. The starting cell's cost is **not** applied before the robot moves.

You want to find the minimum total cost to move the robot to the target cell. However, you **do not know** the grid's dimensions, the starting cell, nor the target cell. You are only allowed to ask queries to the `GridMaster` object.

<!-- more -->

The `GridMaster` class has the following functions:

- `boolean canMove(char direction)` Returns `true` if the robot can move in that direction. Otherwise, it returns `false`.
- `int move(char direction)` Moves the robot in that direction and returns the cost of moving to that cell. If this move would move the robot to a blocked cell or off the grid, the move will be **ignored**, the robot will remain in the same position, and the function will return `-1`.
- `boolean isTarget()` Returns `true` if the robot is currently on the target cell. Otherwise, it returns `false`.

Note that `direction` in the above functions should be a character from `{'U','D','L','R'}`, representing the directions up, down, left, and right, respectively.

Return *the **minimum total cost** to get the robot from its initial starting cell to the target cell. If there is no valid path between the cells, return* `-1`.

**Custom testing:**

The test input is read as a 2D matrix `grid` of size `m x n` and four integers `r1`, `c1`, `r2`, and `c2` where:

- `grid[i][j] == 0` indicates that the cell `(i, j)` is blocked.
- `grid[i][j] >= 1` indicates that the cell `(i, j)` is empty and `grid[i][j]` is the **cost** to move to that cell.
- `(r1, c1)` is the starting cell of the robot.
- `(r2, c2)` is the target cell of the robot.

Remember that you will **not** have this information in your code.



**Example 1:**

```
Input: grid = [[2,3],[1,1]], r1 = 0, c1 = 1, r2 = 1, c2 = 0
Output: 2
Explanation: One possible interaction is described below:
The robot is initially standing on cell (0, 1), denoted by the 3.
- master.canMove('U') returns false.
- master.canMove('D') returns true.
- master.canMove('L') returns true.
- master.canMove('R') returns false.
- master.move('L') moves the robot to the cell (0, 0) and returns 2.
- master.isTarget() returns false.
- master.canMove('U') returns false.
- master.canMove('D') returns true.
- master.canMove('L') returns false.
- master.canMove('R') returns true.
- master.move('D') moves the robot to the cell (1, 0) and returns 1.
- master.isTarget() returns true.
- master.move('L') doesn't move the robot and returns -1.
- master.move('R') moves the robot to the cell (1, 1) and returns 1.
We now know that the target is the cell (1, 0), and the minimum total cost to reach it is 2. 
```

**Example 2:**

```
Input: grid = [[0,3,1],[3,4,2],[1,2,0]], r1 = 2, c1 = 0, r2 = 0, c2 = 2
Output: 9
Explanation: The minimum cost path is (2,0) -> (2,1) -> (1,1) -> (1,2) -> (0,2).
```

**Example 3:**

```
Input: grid = [[1,0],[0,1]], r1 = 0, c1 = 0, r2 = 1, c2 = 1
Output: -1
Explanation: There is no path from the robot to the target cell.
```



**Constraints:**

- `1 <= n, m <= 100`
- `m == grid.length`
- `n == grid[i].length`
- `0 <= grid[i][j] <= 100`

---

## Analysis

&emsp;&emsp;接连两道题目我将给大家分享一下处理hidden grid类型题目。这类题目特点很明显，给出一个class去对一个机器人进行移动，在不知道图确切的细节以及机器人所在的位置前提下，完成最短路径等计算。今天就先借助这道题目来分析下这类型的题目。

&emsp;&emsp;首先题目的要求是计算出从起点到终点的cost最少的路径，返回最少的cost即可。最短路径算法用dijkstra算法就可以了，用一个priority queue做BFS，每次选出cost最小的点连接，然后把未访问过的邻居都加进队列即可。这样后半部分就解决了，问题就剩下我们怎么得到一个图。

&emsp;&emsp;因为前面用的是BFS，使用一个class去操作机器人是很难做到实时的BFS的，因为无法控制机器人的位置，所以这里采取的方法是先把hidden grid通过遍历构造出来。然后相当于我们就有了grid了，再做一次普通的dijkstra算法即可。因此我们focus在如何把grid构建出来。

&emsp;&emsp;先来看我们怎么存grid。我们知道地图的大小最大是`100 * 100`，但是我们不知道起点的坐标是什么。一般来说这种情况我们有两个方法：

1. 用`unordered_map<int, unordered_map<int, int>>`的方法替代二维矩阵，这个的好处是不需要处理越界的问题，访问起来和二维矩阵没什么区别；
2. 用二维矩阵。因为矩阵的大小是100 * 100，我们可以假设起点在中间`(50, 50)`，那么如果实际上这是grid的右边界，那么左边就需要有50列，如果实际上是grid的左边界，那么右边就需要有50列，所以一共是100列就可以cover所有的情况。普遍一点，如果我们定义起点是`(x, x)`，那么矩阵初始化为`2x * 2x`即可，一般`x = m / 2`。

&emsp;&emsp;这里我用第一种方法，下面一题我用第二种方法，两种都尝试一下。然后我们来看如何遍历，前面提到只给一个class这种方法不适合BFS，却非常适合DFS，因为操作的位置是连续的，所以我们只需要每次往4个方向先调用`canMove()`判断是否能移动，然后再移动到该位置，把`cost`记录到上面说的二层`unordered_map`中即可。需要注意，这里有点类似与backtracing，需要撤销操作，也就是说往左移动一步dfs后，要向右移动一步抵消之前的操作，这样robot才能回到原位。

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
 *     int move(char direction);
 *     boolean isTarget();
 * };
 */
struct cmp {
    bool operator()(const vector<int> &a, const vector<int> &b) {
        return a[2] > b[2];
    }
};
class Solution {
public:
    int findShortestPath(GridMaster &master) {
        targetI = INT_MAX, targetJ = INT_MAX;
        dfs(master, 0, 0, 0);
        if (targetI == INT_MAX || targetJ == INT_MAX) {
            return -1;
        }
        visited.clear();
        // bfs
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};
        priority_queue<vector<int>, vector<vector<int>>, cmp> pq;
        pq.push({0, 0, 0});
        while (!pq.empty()) {
            auto cur = pq.top();
            pq.pop();
            int i = cur[0], j = cur[1], cost = cur[2];
            if (i == targetI && j == targetJ) {
                return cost;
            }
            
            for (int k = 0; k < 4; ++k) {
                int newI = i + dx[k];
                int newJ = j + dy[k];
                int key = newI * 101 + newJ;
                if (graph[newI][newJ] > 0 && !visited.count(key)) {
                    pq.push({newI, newJ, graph[newI][newJ] + cost});
                    visited.insert(key);
                }
            }
        }
        return -1;
    }
private:
    unordered_map<int, unordered_map<int, int>> graph;
    int targetI, targetJ;
    unordered_set<int> visited;
    void dfs(GridMaster &master, int i, int j, int cost) {
        graph[i][j] = cost;
        if (master.isTarget()) {
            targetI = i;
            targetJ = j;
            return;
        }
        
        int key = i * 101 + j;
        visited.insert(key);
        
        int left = i * 101 + j - 1, right = i * 101 + j + 1, up = (i - 1) * 101 + j, down = (i + 1) * 101 + j;
        if (master.canMove('L') && !visited.count(left)) {
            int temp = master.move('L');
            dfs(master, i, j - 1, temp);
            master.move('R');
        }
        if (master.canMove('R') && !visited.count(right)) {
            int temp = master.move('R');
            dfs(master, i, j + 1, temp);
            master.move('L');
        }
        if (master.canMove('U') && !visited.count(up)) {
            int temp = master.move('U');
            dfs(master, i - 1, j, temp);
            master.move('D');
        }
        if (master.canMove('D') && !visited.count(down)) {
            int temp = master.move('D');
            dfs(master, i + 1, j, temp);
            master.move('U');
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是hidden grid的第一题，大致上我把这类题目的特点描述了，同时也介绍了主要的解题思路是分开两步，先把grid构造出来，再转化为正常的二维问题操作。然后介绍了如何使用DFS的方法把grid构建出来。这道题目的分享到这里，感谢你的支持！

