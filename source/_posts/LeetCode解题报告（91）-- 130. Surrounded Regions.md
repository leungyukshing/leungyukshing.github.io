---
title: LeetCode解题报告（91）-- 130. Surrounded Regions
tags:
  - LeetCode
date: 2020-06-22 16:40:40
mathjax: true

---

## Problem

Given a 2D board containing `'X'` and `'O'` (**the letter O**), capture all regions surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example:**

```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```

**Explanation:**

Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

------

## Analysis

&emsp;&emsp;题目很好理解，被`X`围住的`O`会变成`X`。实际上可以转化为找出所有被围住的`O`。但是**被围住**这个概念不是很好去定义，从代码层面上判断也会比较复杂，不妨换个思路，我们先看一下**没有被围住**的。没有被围住就说明一定有一个`O`在边上，于是我们可以通过搜索方法找到所有和这个`O`相连的`O`，剩下的`O`就是被围住的了。

------

## Solution

&emsp;&emsp;遍历四条边上的`O`，对于每一个都用DFS，然后将其标记为第三个符号，用于标识这些是没有被围住的。遍历完后，将剩下的`O`都标记为`X`，然后将刚刚标识为没有被围住的重新标记为`O`。

------

## Code

```c++
class Solution {
public:
    void solve(vector<vector<char> >& board) {
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[i].size(); ++j) {
                if ((i == 0 || i == board.size() - 1 || j == 0 || j == board[i].size() - 1) && board[i][j] == 'O')
                    solveDFS(board, i, j);
            }
        }
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[i].size(); ++j) {
                if (board[i][j] == 'O') board[i][j] = 'X';
                if (board[i][j] == '$') board[i][j] = 'O';
            }
        }
    }
    void solveDFS(vector<vector<char> > &board, int i, int j) {
        if (board[i][j] == 'O') {
            board[i][j] = '$';
            if (i > 0 && board[i - 1][j] == 'O') 
                solveDFS(board, i - 1, j);
            if (j < board[i].size() - 1 && board[i][j + 1] == 'O') 
                solveDFS(board, i, j + 1);
            if (i < board.size() - 1 && board[i + 1][j] == 'O') 
                solveDFS(board, i + 1, j);
            if (j > 0 && board[i][j - 1] == 'O') 
                solveDFS(board, i, j - 1);
        }
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目实际上非常简单，算法层面上就是一个简单的DFS，难点在于反向思维：**先找出没有被围住的**。之后的工作就变得非常简单了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
