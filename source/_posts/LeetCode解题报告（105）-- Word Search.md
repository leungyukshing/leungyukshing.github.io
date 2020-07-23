---
title: LeetCode解题报告（105）-- 79. Word Search
tags:
  - LeetCode
date: 2020-07-23 22:12:48
mathjax: true

---

## Problem

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

<!-- more -->

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

**Constraints:**

- `board` and `word` consists only of lowercase and uppercase English letters.
- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `1 <= word.length <= 10^3`

------

## Analysis

&emsp;&emsp;这道题目要求的是单词字母的连接，采用的是DFS。以二维矩阵中的每一个字母开头，往四个方向寻找，每个方向都做DFS。然后记录在`word`上的下标，这样就能判断是否遍历完整个单词了。

------

## Solution

&emsp;&emsp;需要用一个和原矩阵同样大小的`visited`矩阵去记录哪些位置的字母已经被遍历过了。每个方向遍历的时候，记得遍历完之后要把当前的位置的`visited`重新设置为`false`。

------

## Code

```c++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (board.empty() || board[0].empty()) {
            return false;
        }
        int m = board.size(), n = board[0].size();
        
        vector<vector<bool>> visited(m, vector<bool>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (search(board, word, 0, i, j, visited)) return true;
            }
        }
        return false;
    }
    
    bool search(vector<vector<char>>& board, string word, int idx, int i, int j, vector<vector<bool>>& visited) {
        if (idx == word.size()) {
            return true;
        }
        int m = board.size(), n = board[0].size();
        
        if (i < 0 || j < 0 || i >= m || j >= n || visited[i][j] || board[i][j] != word[idx]) return false;
        
        visited[i][j] = true;
        bool res = search(board, word, idx + 1, i - 1, j, visited) 
                 || search(board, word, idx + 1, i + 1, j, visited)
                 || search(board, word, idx + 1, i, j - 1, visited)
                 || search(board, word, idx + 1, i, j + 1, visited);
        visited[i][j] = false;
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的基本思路就是二维矩阵的DFS，往四个方向遍历，难度并不是特别很大，关键要记录好字符串的idx。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
