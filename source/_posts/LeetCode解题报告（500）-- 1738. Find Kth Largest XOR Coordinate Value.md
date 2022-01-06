---
title: LeetCode解题报告（500）-- 1738. Find Kth Largest XOR Coordinate Value
mathjax: true
date: 2022-01-05 21:45:23
---

## Problem

You are given a 2D `matrix` of size `m x n`, consisting of non-negative integers. You are also given an integer `k`.

The **value** of coordinate `(a, b)` of the matrix is the XOR of all `matrix[i][j]` where `0 <= i <= a < m` and `0 <= j <= b < n` **(0-indexed)**.

Find the `kth` largest value **(1-indexed)** of all the coordinates of `matrix`.

<!-- more -->

**Example 1:**

```
Input: matrix = [[5,2],[1,6]], k = 1
Output: 7
Explanation: The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.
```

**Example 2:**

```
Input: matrix = [[5,2],[1,6]], k = 2
Output: 5
Explanation: The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.
```

**Example 3:**

```
Input: matrix = [[5,2],[1,6]], k = 3
Output: 4
Explanation: The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.
```



**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 106`
- `1 <= k <= m * n`

---

## Analysis

&emsp;&emsp;这道题目给出一个矩阵，定义每个位置的`value`是以它和左上角为顶点的矩阵内所有元素的异或结果。对于矩阵中，要求子矩阵的和、异或结果等，优先考虑的就是矩阵前缀和。其基本公式为`preSum[i][j] = preSum[i - 1][j] + preSum[i][j - 1] + preSum[i][j] - preSum[i - 1][j - 1]`。

&emsp;&emsp;这道题目基本就按照上面的公式就能快速求得每个位置的value，然后是求第k大的value。求第`k`大数一般有两种方法，一个是用priority queue，另一种是类似于快排的方法。这里因为value是随着不断产生的，更加适合用priority queue，所以我们维护前`k`个元素即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int kthLargestValue(vector<vector<int>>& matrix, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        int m = matrix.size(), n = matrix[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                int sum = matrix[i][j];
                if (i > 0) {
                    sum ^= matrix[i - 1][j];
                }
                if (j > 0) {
                    sum ^= matrix[i][j - 1];
                }
                if (i > 0 && j > 0) {
                    sum ^= matrix[i - 1][j - 1];
                }
                matrix[i][j] = sum;
                pq.push(sum);
                if (pq.size() > k) {
                    pq.pop();
                }
            }
        }
        return pq.top();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是矩阵preSum加求第`k`大数两个知识点的结合，都不难。这道题目的分享到这里，感谢你的支持！

