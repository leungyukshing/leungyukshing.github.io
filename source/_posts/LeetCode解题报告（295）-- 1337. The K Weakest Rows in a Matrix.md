---
title: LeetCode解题报告（295)-- 1337. The K Weakest Rows in a Matrix
tags:
  - LeetCode
mathjax: true
abbrlink: 322
date: 2021-02-16 12:18:11
---

## Problem

Given a `m * n` matrix `mat` of *ones* (representing soldiers) and *zeros* (representing civilians), return the indexes of the `k` weakest rows in the matrix ordered from the weakest to the strongest.

A row ***i*** is weaker than row ***j***, if the number of soldiers in row ***i*** is less than the number of soldiers in row ***j***, or they have the same number of soldiers but ***i*** is less than ***j***. Soldiers are **always** stand in the frontier of a row, that is, always *ones* may appear first and then *zeros*.

<!-- more -->

**Example 1:**

```
Input: mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
Output: [2,0,3]
Explanation: 
The number of soldiers for each row is: 
row 0 -> 2 
row 1 -> 4 
row 2 -> 1 
row 3 -> 2 
row 4 -> 5 
Rows ordered from the weakest to the strongest are [2,0,3,1,4]
```

**Example 2:**

```
Input: mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
Output: [0,2]
Explanation: 
The number of soldiers for each row is: 
row 0 -> 1 
row 1 -> 4 
row 2 -> 1 
row 3 -> 1 
Rows ordered from the weakest to the strongest are [0,2,3,1]
```

**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `2 <= n, m <= 100`
- `1 <= k <= m`
- `matrix[i][j]` is either 0 **or** 1.

------

## Analysis

&emsp;&emsp;题目给出一个矩阵，我们需要对行进行排序，排序的规则是，行里面1的数量多的靠后，数量少的靠前，最后需要返回行的坐标。解法也比较常规，直接统计每一行1的数量，然后把这个数量和该行对应的下标组成一个pair，放到一个数组中。统计完成后对数组进行排序，最后返回前`k`个即可。

------

## Solution

&emsp;&emsp;在构成pair的时候，需要注意是1的数量放在第一个元素，下标放在第二个元素，这样使用`sort()`的时候默认是对第一个元素进行排序。

------

## Code

```c++
class Solution {
public:
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        vector<vector<int>> rows;
        int m = mat.size();
        int n = mat[0].size();
        
        for (int i = 0; i < m; i++) {
            int sum = 0;
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 1) {
                    sum++;
                }
            }
            rows.push_back({sum, i});
        }
        
        sort(rows.begin(), rows.end());
        vector<int> result;
        for (int i = 0; i < k; i++) {
            result.push_back(rows[i][1]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
