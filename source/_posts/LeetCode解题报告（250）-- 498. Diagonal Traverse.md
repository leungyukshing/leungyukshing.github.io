---
title: LeetCode解题报告（250）-- 498. Diagonal Traverse
tags:
  - LeetCode
mathjax: true
abbrlink: 51290
date: 2020-12-26 02:56:24
---

## Problem

Given a matrix of M x N elements (M rows, N columns), return all elements of the matrix in diagonal order as shown in the below image.

<!-- more -->

**Example:**

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

Output:  [1,2,4,7,5,3,6,8,9]
```

**Explanation:**

<img src="https://assets.leetcode.com/uploads/2018/10/12/diagonal_traverse.png" alt="Explanation" style="zoom:48%;" />

**Note:**

The total number of elements of the given matrix will not exceed 10,000.

------

## Analysis

&emsp;&emsp;这道题是对一个二维数组进行对角线遍历。因为所有的元素都要遍历，而且矩阵的内容本身是没有限定的，所以还是得乖乖地按着题目给定的方法来遍历。

&emsp;&emsp;对角线遍历实际上是有两个方向，一个是往左下，另一个是往右上，这个的实现是比较容易的，都是横纵坐标的加减。有了方向后，第二个看转折点，转折点也是比较好找的，去到边界就是转折。这道题目的难点就在于转折后横纵坐标的变化，我们先看横坐标越界的情况，如3，遍历完3后，横坐标大于`n`，所以下一个6就是取`n - 1`，纵坐标因为也往上走了一个，所以下一个到6的话需要+2；纵坐标越界的情况也是这样。

------

## Solution

&emsp;&emsp;用一个遍历控制方向，预先写好两个方向`delta = {{-1, 1}, {1, -1}}`，这样后面更新坐标比较方便。

------

## Code

```c++
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) {
        int m = matrix.size();
        if (m == 0) {
            return {};
        }
        int n = matrix[0].size();
        
        int r = 0, c = 0, dir = 0;
        vector<vector<int>> delta{{-1, 1}, {1, -1}};
        vector<int> result(m * n);
        for (int i = 0; i < m * n; i++) {
            result[i] = matrix[r][c];
            r += delta[dir][0];
            c += delta[dir][1];
            if (r >= m) {
                r = m - 1;
                c += 2;
                dir = 1 - dir;
            }
            if (c >= n) {
                c = n - 1;
                r += 2;
                dir = 1 - dir;
            }
            if (r < 0) {
                r = 0;
                dir = 1 - dir;
            }
            if (c < 0) {
                c = 0;
                dir = 1 - dir;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质还是二维数组的遍历，但是增加了一些难度。和螺旋遍历一样，对角线遍历实际上考察的还是对下标的熟悉度，这类题目没有特别巧妙的方法，自己拿一个例子对这算出规律就好。这道题目的分享到这里，谢谢您的支持！
