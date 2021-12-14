---
title: LeetCode解题报告（426）-- 723. Candy Crush
mathjax: true
date: 2021-12-14 15:47:21
---

## Problem

This question is about implementing a basic elimination algorithm for Candy Crush.

Given an `m x n` integer array `board` representing the grid of candy where `board[i][j]` represents the type of candy. A value of `board[i][j] == 0` represents that the cell is empty.

The given board represents the state of the game following the player's move. Now, you need to restore the board to a stable state by crushing candies according to the following rules:

- If three or more candies of the same type are adjacent vertically or horizontally, crush them all at the same time - these positions become empty.
- After crushing all candies simultaneously, if an empty space on the board has candies on top of itself, then these candies will drop until they hit a candy or bottom at the same time. No new candies will drop outside the top boundary.
- After the above steps, there may exist more candies that can be crushed. If so, you need to repeat the above steps.
- If there does not exist more candies that can be crushed (i.e., the board is stable), then return the current board.

You need to perform the above rules until the board becomes stable, then return *the stable board*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/10/12/candy_crush_example_2.png)

```
Input: board = [[110,5,112,113,114],[210,211,5,213,214],[310,311,3,313,314],[410,411,412,5,414],[5,1,512,3,3],[610,4,1,613,614],[710,1,2,713,714],[810,1,2,1,1],[1,1,2,2,2],[4,1,4,4,1014]]
Output: [[0,0,0,0,0],[0,0,0,0,0],[0,0,0,0,0],[110,0,0,0,114],[210,0,0,0,214],[310,0,0,113,314],[410,0,0,213,414],[610,211,112,313,614],[710,311,412,613,714],[810,411,512,713,1014]]
```

**Example 2:**

```
Input: board = [[1,3,5,5,2],[3,4,3,3,1],[3,2,4,5,2],[2,4,4,5,5],[1,4,4,1,1]]
Output: [[1,3,0,0,0],[3,4,0,5,2],[3,2,0,3,1],[2,4,0,5,2],[1,4,3,1,1]]
```

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `3 <= m, n <= 50`
- `1 <= board[i][j] <= 2000`

------

## Analysis

&emsp;&emsp;题目是一个消消乐的游戏，给出一个二维矩阵，定义水平方向和竖直方向上连续三个或以上就可以消除掉，消除后上面的元素就要往下掉。于是这道题目就分为两个步骤：

1. 先进行消除，我们检测每一列和每一行，碰到有大于等于连续三个一样的就消除掉。需要注意，同一个位置可以同时被水平方向和竖直方向利用，所以这里采用绝对值的方式进行判断。即，消除某个元素先不置为0，而是更改成相反数，然后判断是否相同就用绝对值；
2. 然后再处理掉落的情况。二维矩阵中处理掉落的情况是有套路的，一般是用双指针，一个`j`用于遍历，方向是从下到上，另一个`wl`用于指向下一个能够放置的位置。当某个位置的值大于0，说明这个值是要保留的，否则就跳过。最后把`wl`以上的位置全部置为0.

&emsp;&emsp;上面就是基本的更新思路，因为这道题目要求的时返回消除后的最终状态，所以如果还有可以消除的话，就要递归调用。我们可以在第一步的时候加上一个变量，表示当前有没有消除，如果没有，则可以直接返回结果，否则继续调用。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> candyCrush(vector<vector<int>>& board) {
        int m = board.size(), n = board[0].size();
        
        bool updated = false;
        
        // detect rows
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n - 2; ++j) {
                int val = abs(board[i][j]);
                if (board[i][j] != 0 && abs(board[i][j + 1]) == val && abs(board[i][j + 2]) == val) {
                    board[i][j] = board[i][j + 1] = board[i][j + 2] = -val;
                    updated = true;
                }
            }
        }
        
        // detect columns
        for (int j = 0; j < n; ++j) {
            for (int i = 0; i < m - 2; ++i) {
                int val = abs(board[i][j]);
                if (board[i][j] != 0 && abs(board[i + 1][j]) == val && abs(board[i + 2][j]) == val) {
                    board[i][j] = board[i + 1][j] = board[i + 2][j] = -val;
                    updated = true;
                }
            }
        }
        
        // update
        for (int j = 0; j < n; j++) {
            int wl = m - 1;
            for (int i = m - 1; i >= 0; i--) {
                if (board[i][j] > 0) {
                   board[wl--][j] = board[i][j];
                }
            }
            while (wl >= 0) {
                board[wl--][j] = 0;
            }
        }
        
        return updated ? candyCrush(board): board;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是很经典的二维矩阵消除问题，其基本思想是双指针。这道题目的分享到这里，感谢你的支持！
