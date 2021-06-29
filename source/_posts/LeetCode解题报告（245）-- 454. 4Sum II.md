---
title: LeetCode解题报告（245）-- 454. 4Sum II
tags:
  - LeetCode
mathjax: true
abbrlink: 31499
date: 2020-12-18 01:00:04
---

## Problem

Given four lists A, B, C, D of integer values, compute how many tuples `(i, j, k, l)` there are such that `A[i] + B[j] + C[k] + D[l]` is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

<!-- more -->

**Example:**

```
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

------

## Analysis

&emsp;&emsp;这是一道4Sum的系列题，实际上和2Sum的关系更大。如果暴力求解的话，就是一个四重循环，复杂度为$O(n^4)$。既然2Sum可以降低复杂度，这道题目是否也可以采取类似的思路呢？当然是可以的。我们先处理A、B，把他们的各种组合的和记录到一个map中，然后再处理C、D计算出他们的各种组合的和，然后取**相反数**，看看能不能在map中找到。如果能找到，说明四个数字之和为0，答案数量+1。

------

## Solution

&emsp;&emsp;实际上是2Sum的一个扩展，还是使用map。

------

## Code

```c++
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> m;
        int result = 0;
        
        for (int i = 0; i < A.size(); i++) {
            for (int j = 0; j < B.size(); j++) {
                m[A[i] + B[j]]++;
            }
        }
        
        for (int i = 0; i < C.size(); i++) {
            for (int j = 0; j < D.size(); j++) {
                result += m[-(C[i] + D[j])];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是2Sum的一个扩展，实际上它考察了对2Sum的理解。这道题目降低复杂度的思路实际上是先两两合并，转化为2Sum求解。这道题目的分享到这里，谢谢您的支持！
