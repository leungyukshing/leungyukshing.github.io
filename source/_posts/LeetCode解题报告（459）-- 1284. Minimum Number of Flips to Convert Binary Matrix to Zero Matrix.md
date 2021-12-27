---
title: LeetCode解题报告（459）-- 1284. Minimum Number of Flips to Convert Binary Matrix to Zero Matrix
mathjax: true
date: 2021-12-26 21:44:38
---

## Problem

Given a `m x n` binary matrix `mat`. In one step, you can choose one cell and flip it and all the four neighbors of it if they exist (Flip is changing `1` to `0` and `0` to `1`). A pair of cells are called neighbors if they share one edge.

Return the *minimum number of steps* required to convert `mat` to a zero matrix or `-1` if you cannot.

A **binary matrix** is a matrix with all cells equal to `0` or `1` only.

A **zero matrix** is a matrix with all cells equal to `0`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/11/28/matrix.png)

```
Input: mat = [[0,0],[0,1]]
Output: 3
Explanation: One possible solution is to flip (1, 0) then (0, 1) and finally (1, 1) as shown.
```

**Example 2:**

```
Input: mat = [[0]]
Output: 0
Explanation: Given matrix is a zero matrix. We do not need to change it.
```

**Example 3:**

```
Input: mat = [[1,0,0],[1,0,0]]
Output: -1
Explanation: Given matrix cannot be a zero matrix.
```



**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 3`
- `mat[i][j]` is either `0` or `1`.

---

## Analysis

&emsp;&emsp;题目给出一个只包含01的矩阵，每次允许的操作是对一个位置以及它的邻居位置进行flip，即0变成1，1变成0。问最少需要多少步能够使得整个矩阵全零。一般这种状态转移，而且是要最短的，会首先思考BFS，因为BFS天然就给了一种最短的路径，而且01矩阵很适合编码成一个状态。

&emsp;&emsp;我们先不谈编码，假设我们把每个矩阵都看作一种状态，那么我们对这个状态做BFS，下一个状态就是遍历矩阵的所有位置，把每个位置都做一次flip（当然包括它的邻居），我们用一个`unordered_set`来记录已经遍历过的状态避免死循环。这样如果能找到一条路径的话，我们一定会遇到一个全零的矩阵，否则就退出循环，返回-1。

&emsp;&emsp;接下来我们再来看状态编码。为什么我说01矩阵特别适合编码，首先矩阵可以用一个一维的数组表示，相信大家对这个都不陌生，考虑到值只能是0或1，所以我们可以直接用一个二进制串来表示。题目限定了矩阵的大小最大就是3 x 3，所以用一个int绝对是够的。之后我们的flip都可以采用位运算进行。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minFlips(vector<vector<int>>& mat) {
        // from matrix to bitVec
        int bitVec = 0;
        m = mat.size(), n = mat[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                bitVec <<= 1;
                bitVec |= mat[i][j];
            }
        }
        

        
        queue<int> q;
        unordered_set<int> visited;
        int step = 0;
        q.push(bitVec);
        visited.insert(bitVec);
        
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int cur = q.front();
                q.pop();
                if (cur == 0) {
                    return step;
                }
                
                // try every position
                for (int i = 0; i < m; ++i) {
                    for (int j = 0; j < n; ++j) {
                        int next = helper(i, j, cur);
                        if (!visited.count(next)) {
                            visited.insert(next);
                            q.push(next);
                        }
                    }
                }
            }
            
            step++;
        }
        return -1;
    }
private:
    int m, n;
    int helper(int i, int j, int cur) {
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, -1, 0, 1};
        // flip [i, j]
        int pos = m * n - 1 - i * n - j;
        cur ^= 1 << pos;
        
        // flip neighbors
        for (int k = 0; k < 4; ++k) {
            int newX = i + dx[k];
            int newY = j + dy[k];
            if (newX < 0 || newX >= m || newY < 0 || newY >= n) {
                continue;
            }
            pos = m * n - 1 - newX * n - newY;
            cur ^= 1 << pos;
        }
        return cur;
    }
};
```

------

## Summary

&emsp;&emsp;这道问题实际上就是一个BFS，但是难点有二，第一个就是找到BFS这个解法，对于状态转移而且有很多可能的，又是找最短路径，要优先考虑BFS；第二个就是状态编码，对于01矩阵要有这种敏感度。这道题目的分享到这里，感谢你的支持！
