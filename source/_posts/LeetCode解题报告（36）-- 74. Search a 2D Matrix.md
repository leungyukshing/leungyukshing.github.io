---
title: LeetCode解题报告（36）-- 74. Search a 2D Matrix
tags:
  - LeetCode
abbrlink: 30693
date: 2018-11-19 16:00:23
---
## Problem
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
  + Integers in each row are sorted from left to right.
  + The first integer of each row is greater than the last integer of the previous row.
<!-- more -->

**Example 1:**
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

**Example 2:**
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

## Analysis
&emsp;&emsp;这道题是一道关于搜索算法的题目，在给定的一个矩阵中，搜索`target`是否存在。最直接的做法就是双重循环遍历数组，但是这样的做法显然是低效的。我们可以利用矩阵本身的一些特性来优化搜索的算法。
&emsp;&emsp;根据题目的描述，我们可以总结出，数组中**行内是有序的**，且**行间也是有序的**。我们可以通过比较每一行中的最后一个元素来定位`target`可能存在的行，然后再对该行遍历就可以了。

## Solution
&emsp;&emsp;如果`target`在第`i`行中，那么它一定满足`target > matrix[i-1][n-1] && target <= matrix[i][n-1]`。这里需要注意特殊情况，即`target`在第一行。同时，还要注意越界的情况。
&emsp;&emsp;注意`row`初始化为`INT_MAX`，这里保证如果`target`比矩阵中所有的数都大时，我们能够判断出来。

## Code
```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if (m == 0)
            return false;
        int n = matrix[0].size();
        if (n == 0)
            return false;
        // target in first row
        if (target <= matrix[0][n-1]) {
            for (int i = 0; i < n; i++) {
                if (target == matrix[0][i])
                    return true;
            }
            return false;
        }

        if (m == 1) {
            return false;
        }

        int row = INT_MAX;
        for (int i = 0; i < m-1; i++) {
            if (target > matrix[i][n-1] && target <= matrix[i+1][n-1]) {
                row = i+1;
                break;
            }
        }

        if (row >= m) {
            return false;
        }

        for (int i = 0; i < n; i++) {
            if (target == matrix[row][i]) {
                return true;
            }
        }
        return false;
    }
};
```
**运行时间：**约8ms，超过97.55%的CPP代码。

## Summary
&emsp;&emsp;这道题是关于搜索算法比较新颖的一题，以往一般会使用二分查找来提高搜索效率。其实这题也是利用了同样的思想，利用矩阵本身的有序性，对比关键元素的值，缩窄搜索范围，最终搜得结果。这道题的分析到此为止，谢谢您的支持！
