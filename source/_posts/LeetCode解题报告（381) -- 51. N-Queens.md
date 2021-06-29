---
title: LeetCode解题报告（381) -- 51. N-Queens
tags:
  - LeetCode
mathjax: true
abbrlink: 40023
date: 2021-06-05 17:17:51
---

## Problem

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

**Example 2:**

```
Input: n = 1
Output: [["Q"]]
```



**Constraints:**

- `1 <= n <= 9`

------

## Analysis

&emsp;&emsp;这是一道经典的N皇后问题，在一个`n * n`的棋盘上放置`n`个皇后使得他们能相互存活，这类问题的经典解法就是递归+回溯。基本思想就是我先试一下放这个位置行不行，如果不行的话我再放其他位置试试。

 &emsp;&emsp;首先我们要确定终止条件，应该是遍历到了最后一行时，就说明已经都放好了。然后我们需要解决最难的地方，如何判断位置是否有冲突。这块逻辑可以抽取出来，单独作为一个函数进行判断，我们要判断同一行、同一列、以及两个对角线上是否存在皇后，如果存在则冲突。特别注意递归前放置一个皇后，递归结束后要重置状态，把皇后移走。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<string> queens(n, string(n, '.'));
        helper(0, queens, res);
        return res;
    }
    void helper(int curRow, vector<string>& queens, vector<vector<string>>& res) {
        int n = queens.size();
        if (curRow == n) {
            res.push_back(queens);
            return;
        }
        for (int i = 0; i < n; ++i) {
            if (isValid(queens, curRow, i)) {
                queens[curRow][i] = 'Q';
                helper(curRow + 1, queens, res);
                queens[curRow][i] = '.';
            }
        }
    }
    bool isValid(vector<string>& queens, int row, int col) {
        for (int i = 0; i < row; ++i) {
            if (queens[i][col] == 'Q') return false;
        }
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j) {
            if (queens[i][j] == 'Q') return false;
        }
        for (int i = row - 1, j = col + 1; i >= 0 && j < queens.size(); --i, ++j) {
            if (queens[i][j] == 'Q') return false;
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是一道经典的算法题，把里面每个细节都吃透后，递归回溯类的题目基本就能掌握了。这道题目的分享到这里，感谢你的支持！
