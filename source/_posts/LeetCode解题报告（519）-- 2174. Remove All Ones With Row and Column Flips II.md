---
title: LeetCode解题报告（519）-- 2174. Remove All Ones With Row and Column Flips II
mathjax: true
date: 2022-04-13 16:01:23



---

## Problem

You are given a **0-indexed** `m x n` **binary** matrix `grid`.

In one operation, you can choose any `i` and `j` that meet the following conditions:

- `0 <= i < m`
- `0 <= j < n`
- `grid[i][j] == 1`

and change the values of **all** cells in row `i` and column `j` to zero.

Return *the **minimum** number of operations needed to remove all* `1`*'s from* `grid`*.*

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2022/02/13/image-20220213162716-1.png)

```
Input: grid = [[1,1,1],[1,1,1],[0,1,0]]
Output: 2
Explanation:
In the first operation, change all cell values of row 1 and column 1 to zero.
In the second operation, change all cell values of row 0 and column 0 to zero.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2022/02/13/image-20220213162737-2.png)

```
Input: grid = [[0,1,0],[1,0,1],[0,1,0]]
Output: 2
Explanation:
In the first operation, change all cell values of row 1 and column 0 to zero.
In the second operation, change all cell values of row 2 and column 1 to zero.
Note that we cannot perform an operation using row 1 and column 1 because grid[1][1] != 1.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2022/02/13/image-20220213162752-3.png)

```
Input: grid = [[0,0],[0,0]]
Output: 0
Explanation:
There are no 1's to remove so return 0.
```



**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 15`
- `1 <= m * n <= 15`
- `grid[i][j]` is either `0` or `1`.

---

## Analysis

&emsp;&emsp;这道题是[Remove All Ones With Row and Column Flips](https://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88518%EF%BC%89--%202128.%20Remove%20All%20Ones%20With%20Row%20and%20Column%20Flips.html)的系列题，但是和之前这题的做法截然不同。前面提到了flipping的特点是只flip一次，flip两次都多余。但是这道题目是对所有的位置为1的格子，对其所在行和所在列变更为0（注意这里不是flip了，而是直接变成0），所以之前的性质在这里就不管用了。所以要变换思路，第一个提示是题目要求的是minimum operations，这种明显的提示一般就是BFS或者memorized searching，那我们就按照这个思路去做。

&emsp;&emsp;首先要解决的是状态问题，每一个状态都是一个矩阵，这样非常不利于我们做BFS或DFS。对于01矩阵来说，一个常见的做法就是进行编码，把矩阵打平成一维，然后利用位运算，使用一个32位整数表示即可。这里一个前提是题目给定了矩阵的大小不超过15。

&emsp;&emsp;搜索方法这里我采用的是DFS+记忆数组。对每个位置的更新都是通过位运算完成，记忆数组`memo[i]`表示的是`i`这个状态下变成0需要的最少的操作数量。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int removeOnes(vector<vector<int>>& grid) {
        memo = vector<int>(32768, INT_MAX);
        int state = 0;
        m = grid.size(), n = grid[0].size();
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    state |= (1 << (i * n + j));
                }
            }
        }
        return dfs(state);
    }
private:
    int m, n;
    vector<int> memo;
    int dfs(int state) {
        if (state == 0) {
            return 0;
        }
        if (memo[state] != INT_MAX) {
            return memo[state];
        }
        
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (state & (1 << (i * n + j))) {
                    int newState = state;
                    for (int k = 0; k < m; ++k) {
                        newState &= ~(1 << (k * n + j));
                    }
                    for (int k = 0; k < n; ++k) {
                        newState &= ~(1 << (i * n + k));
                    }
                    memo[state] = min(memo[state], 1 + dfs(newState));
                }
            }
        }
        return memo[state];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上就是BFS或DFS的应用，难点在于对矩阵进行编码。这道题目的分享到这里，感谢你的支持！
