---
title: LeetCode解题报告（255）-- 289. Game of Life
tags:
  - LeetCode
mathjax: true
date: 2020-12-30 21:37:15

---

## Problem

According to [Wikipedia's article](https://en.wikipedia.org/wiki/Conway's_Game_of_Life): "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an `m x n` grid of cells, where each cell has an initial state: **live** (represented by a `1`) or **dead** (represented by a `0`). Each cell interacts with its [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population.
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the `m x n` grid `board`, return *the next state*.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/12/26/grid1.jpg)

```
Input: board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
Output: [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```

**Example 2:**

![Example 2](https://assets.leetcode.com/uploads/2020/12/26/grid2.jpg)

```
Input: board = [[1,1],[1,0]]
Output: [[1,1],[1,1]]
```

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 25`
- `board[i][j]` is `0` or `1`. 

**Follow up:**

- Could you solve it in-place? Remember that the board needs to be updated simultaneously: You cannot update some cells first and then use their updated values to update other cells.
- In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches upon the border of the array (i.e., live cells reach the border). How would you address these problems?

------

## Analysis

&emsp;&emsp;这道题目是康威游戏的实现。游戏给出一个二维平面图，每个位置代表一个细胞，0代表死细胞，1代表活细胞，每一代细胞的更新遵循以下规则（细胞是同时更新的）：

1. 如果一个活细胞周围的八个位置中的活细胞少于两个，该细胞变成死细胞；
2. 如果一个活细胞周围的八个位置中有两个或三个活细胞，该细胞仍然是活细胞；
3. 如果一个活细胞周围的八个位置中有超过三个活细胞，该细胞变成死细胞；
4. 如果一个死细胞周围的八个位置中正好有三个活细胞，该细胞变成活细胞。

&emsp;&emsp;这个规则看上去也是非常简单，只需要对八个位置做一个统计即可。难点在于题目要求是in-place操作，也就是说不能使用额外的空间。所以问题就变得棘手，因为一旦更新了某个细胞的状态后，其他位置的更新就不知道原来的状态了。所以我们要解决的就是在更新完之后，怎么才能知道原来的状态呢？这里就要用到状态转换机，一共有四种状态：

1. 死细胞->死细胞
2. 死细胞->活细胞
3. 活细胞->死细胞
4. 活细胞->活细胞

&emsp;&emsp;这里就是解决这道题目最巧妙的地方，要把这四种状态和题目给出的条件结合起来。与初始条件对应：

- 死细胞->死细胞对应的状态值是0；
- 活细胞->活细胞对应的状态值是1；
- 活细胞->死细胞对应的状态值是2；
- 死细胞->活细胞对应的状态值是3。

&emsp;&emsp; 这样设定的好处是，当遍历某个位置的时候，如果因为我们要计算它邻居原来活细胞的个数，所以我们只需要统计状态1和状态2即可。接下来我们要考虑最后如何把这四种状态转回两种状态，可以看到上面0和2最后都是死细胞，而1和3最后都是活细胞，这里只需要一个简单的取余就可以做到。

------

## Solution

&emsp;&emsp;每个细胞都要遍历周围的八个位置，可以预先把坐标的delta写成数组，然后统计计算即可。

------

## Code

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        if (m == 0) {
            return;
        }
        int n = board[0].size();
        
        vector<int> dx{-1, -1, -1, 0, 1, 1, 1, 0};
        vector<int> dy{-1, 0, 1, 1, 1, 0, -1, -1};
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int count = 0; // live cell
                for (int k = 0; k < 8; k++) {
                    int x = i + dx[k];
                    int y = j + dy[k];
                    if (x >= 0 && x < m && y >= 0 && y < n && (board[x][y] == 1 || board[x][y] == 2)) {
                        count++;
                    }
                }
                if (board[i][j] && (count < 2 || count > 3)) {
                    board[i][j] = 2;
                } else if (!board[i][j] && count == 3) {
                    board[i][j] = 3;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] %= 2;
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质上是二维数组的遍历，难点在于状态的转换。先从两个状态变为四个状态，再从四个状态变回两个状态。状态机的应用是非常广泛的，这里也算是开拓一下思路，在日常的编码中，我们也经常会用到状态转换机。这道题这道题目的分享到这里，谢谢您的支持！
