---
title: LeetCode解题报告（386) -- 52. N-Queens II
tags:
  - LeetCode
mathjax: true
abbrlink: 51291
date: 2021-06-06 16:29:40
---

## Problem

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *the number of distinct solutions to the **n-queens puzzle***.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
```

**Example 2:**

```
Input: n = 1
Output: 1
```



**Constraints:**

- `1 <= n <= 9`

------

## Analysis

&emsp;&emsp;还是一道N皇后问题，但是比之前那道明显简单了。题目背景还是一样的，上次的题目要求我们返回的是整个位置，需要标明皇后的位置；但这道题目只需要我们返回有多少种方案，所以我们只需要计数即可。

&emsp;&emsp;还是采用递归+回溯的方法，当我们检测到`row == n`时说明已经有一种可行的摆法，这个时候计数+1，最后总的计数就是答案。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int totalNQueens(int n) {
        int res = 0;
        vector<int> pos(n, -1);
        helper(pos, 0, res);
        return res;
    }
    void helper(vector<int>& pos, int row, int& res) {
        int n = pos.size();
        if (row == n) ++res;
        for (int col = 0; col < n; ++col) {
            if (isValid(pos, row, col)) {
                pos[row] = col;
                helper(pos, row + 1, res);
                pos[row] = -1;
            }
        }
    }
    bool isValid(vector<int>& pos, int row, int col) {
        for (int i = 0; i < row; ++i) {
            if (col == pos[i] || abs(row - i) == abs(col - pos[i])) {
                return false;
            }
        }
        return true;
    }

};
```

------

## Summary

&emsp;&emsp;这道题是N皇后问题的简化版，只要求计算出多少种可行的放置方法，而不需要具体的位置。这道题目的分享到这里，感谢你的支持！
