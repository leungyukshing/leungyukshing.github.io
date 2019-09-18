---
title: LeetCode解题报告（52）-- 419. Battleships in a Board
tags:
  - LeetCode
date: 2019-09-18 10:45:18


---

## Problem

Given an 2D board, count how many battleships are in it. The battleships are represented with `'X'`s, empty slots are represented with `'.'`s. You may assume the following rules:

- You receive a valid board, made of only battleships or empty slots.
- Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape `1xN` (1 row, N columns) or `Nx1` (N rows, 1 column), where N can be of any size.
- At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.

<!-- more -->

**Example:**

```
X..X
...X
...X
```

In the above board there are 2 battleships.

**Invalid Example:**

```
...X
XXXX
...X
```

This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

**Follow up:**

Could you do it in **one-pass**, using only **O(1) extra memory**and **without modifying** the value of the board?

------

## Analysis

&emsp;&emsp;这道题是关于矩阵的，题目定义battleships是**竖直方向**或**水平方向**相连的`X`，所以battleship一定是直线的，不能是曲折的。我们要计算battleship的数量，可以转化为计算battleship头的数量。如何确定是头呢？因为battleship都是直线，所以我们可以对头的定义为：

1. 如果是竖直方向，头的定义是上边没有`X`;
2. 如果是水平方向，头的定义是左边没有`X`

------

## Solution

&emsp;&emsp;在题目的**Follow up**中希望我们做到的是一次遍历且仅使用**O(1)**空间。我们只需要遍历矩阵，逐个位置判断，如果该位置是头，则计数+1。注意判断边界情况，即头有可能是在第一列或第一行。这道题排除了invalid的case，所以做起来比较简单。

------

## Code

```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int result = 0;
        int m = board.size();
        int n = board[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'X' && (i == 0 || board[i - 1][j] != 'X') && (j == 0 || board[i][j - 1] != 'X')) {
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

&emsp;&emsp;这是一道与矩阵有关的题目，考察的主要是对题目的理解能力和信息提取能力。难点在于将battleship的数量转化为计算头的数量，然后对边界情况特殊处理。这道题的分享到这里，欢迎转发、评论，谢谢您的支持！
