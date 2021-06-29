---
title: LeetCode解题报告（90）-- 174. Dungeon Game
tags:
  - LeetCode
mathjax: true
abbrlink: 15742
date: 2020-06-22 14:43:11
---

## Problem

The demons had captured the princess (**P**) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (**K**) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (*negative* integers) upon entering these rooms; other rooms are either empty (*0's*) or contain magic orbs that increase the knight's health (*positive* integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.

**Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.**

<!-- more -->

For example, given the dungeon below, the initial health of the knight must be at least **7** if he follows the optimal path `RIGHT-> RIGHT -> DOWN -> DOWN`.

| -2 (K) | -3   | 3      |
| ------ | ---- | ------ |
| -5     | -10  | 1      |
| 10     | 30   | -5 (P) |

**Note:**

- The knight's health has no upper bound.
- Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

------

## Analysis

&emsp;&emsp;看到这种在矩阵中行走的题目基本上都可以优先考虑dp，但是这道题目的dp并没有这么容易找到。首先看到题目给出了一个限制条件：**只能向右或向下走**。也就是说移动的方向是固定的，这就是很典型的dp限制。

&emsp;&emsp;接着来看一下dp的方向，一般来说会从起点开始，一直到终点。这道题目要求的是**最小的起始值**，而不是最小的花费，这种情况下如果从起点开始，连初始值都不知道是多少，所以我们就尝试反向去做dp，从终点开始倒推。定义状态转移方程`dp[i][j]`为从(i, j)位置到终点的所需要最少的起始生命值。按照这个方程`dp[0][0]`就是我们要求的答案了。

&emsp;&emsp;然后来看一下状态转移具体怎么做。当前位置的生命值实际上是由下一步决定的。比如说下一步向右走，当前格子扣了10滴血，那么我当前位置的生命值至少就应该是`右边格子的生命值 + 10`。所以这里的表达式就是`min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]`（注意扣血的时候`dungeon`是负值）。

&emsp;&emsp;最后看一下边界case。首先是骑士存活的条件是1而不是0，所以要保证最少的初始化血量是1。然后为了遍历时的通用性，申请的dp矩阵多一行和多一列。因此初始化的时候就要把终点的下方和右方两个格子都初始化为1。

------

## Solution

&emsp;&emsp;使用一个二维数组存dp状态。为了解决边界问题申请多一行和多一列，方便遍历。

------

## Code

```c++
class Solution {
public:
    int calculateMinimumHP(vector<vector<int>>& dungeon) {
        int m = dungeon.size();
        int n = dungeon[0].size();
        
        vector< vector<int> > dp(m + 1, vector<int>(n + 1, INT_MAX));
        dp[m][n - 1] = 1;
        dp[m - 1][n] = 1;
        
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                dp[i][j] = max(1, min(dp[i + 1][j], dp[i][ j + 1]) - dungeon[i][j]);
            }
        }
        
        return dp[0][0];
    }
};
```

------

## Summary

 &emsp;&emsp;虽然这也是一道二维dp的题目，但是其难点在于需要逆向思考，从终点往前推算，难度就有所提升了。但只要把握好转移过程中我们要的究竟是最大值还是最小值，coding起来还是很快的。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
