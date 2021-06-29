---
title: LeetCode解题报告（300)-- 240. Search a 2D Matrix II
tags:
  - LeetCode
mathjax: true
abbrlink: 11868
date: 2021-02-24 01:24:17
---

## Problem

Write an efficient algorithm that searches for a `target` value in an `m x n` integer `matrix`. The `matrix` has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

**Example 2:**

![Example 2](https://assets.leetcode.com/uploads/2020/11/24/searchgrid.jpg)

```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
Output: false
```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matix[i][j] <= 109`
- All the integers in each row are **sorted** in ascending order.
- All the integers in each column are **sorted** in ascending order.
- `-109 <= target <= 109`

------

## Analysis

&emsp;&emsp;这是一道系列题，上一道题目的矩阵是按行递增，但这里有不一样，它是向右、下两个方向递增的，但是这两个方向并没有说谁大谁小，所以搜索起来会比较麻烦。这道题目巧妙的地方在于遍历的起点并不是左上角，而是左下角（或右上角）。我们观察Example 1中的左下角元素18，比它大的元素都在它的右侧，比它小的元素都在它的上方。这里就是我们要找的规律，对于矩阵中的元素来说，它的上方一定比它小，而它的右方一定比它大，所以我们就借助这个特点，从左下角开始遍历。如果越界说明找不到元素了，直接return false即可。z

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();
        int i = m - 1, j = 0;
        while (i != -1 && j != n) {
            if (target == matrix[i][j]) {
                return true;
            } else if (target < matrix[i][j]) {
                i--;
            } else {
                j++;
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目巧妙的地方在于它遍历的起点是左下角，这与常规从左上角遍历是很不一样的，所以当题目给出的矩阵有一定的特点之后，我不妨从四个顶点都试一下，看看有没有一些可以利用的性质。这道题目的分享到这里，谢谢您的支持！
