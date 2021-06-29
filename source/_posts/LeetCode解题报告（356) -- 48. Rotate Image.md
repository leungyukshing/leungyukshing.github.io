---
title: LeetCode解题报告（356) -- 48. Rotate Image
tags:
  - LeetCode
mathjax: true
abbrlink: 19879
date: 2021-05-01 14:31:53
---

## Problem

You are given an *n* x *n* 2D `matrix` representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

**Example 3:**

```
Input: matrix = [[1]]
Output: [[1]]
```

**Example 4:**

```
Input: matrix = [[1,2],[3,4]]
Output: [[3,1],[4,2]]
```

**Constraints:**

- `matrix.length == n`
- `matrix[i].length == n`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

------

## Analysis

&emsp;&emsp;题目要求把一个矩阵顺时针旋转90度，而且不允许使用额外的空间。这就要求我们要进行in-place的操作。这里直接提供一个思路，首先按照对角线，把矩阵分成上下两部分，然后交换这两部分，之后再把每一行反转就能得到结果了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int m = matrix.size();
        for (int i = 0; i < m; i++) {
            for (int j = i + 1; j < m; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
            reverse(matrix[i].begin(), matrix[i].end());
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目提供了一个在变换矩阵时一个很重要的思路，就是借助对角线。这道题目的分享到这里，感谢你的支持！
