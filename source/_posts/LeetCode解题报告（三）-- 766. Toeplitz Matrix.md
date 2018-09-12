---
title: LeetCode解题报告（三）-- 766. Toeplitz Matrix
tags:
  - LeetCode
abbrlink: 65432
date: 2018-09-05 21:11:25
---
## Problem
A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an `M x N` matrix, return `True` if and only if the matrix is Toeplitz.

**Example1**:
```
Input:
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
Output: True
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.
```

**Example2**:
```
Input:
matrix = [
  [1,2],
  [2,2]
]
Output: False
Explanation:
The diagonal "[1, 2]" has different elements.
```

**Note**:
  1. `matrix` will be a 2D array of integers.
  2. `matrix` will have a number of rows and columns in range `[1, 20]`.
  3. `matrix[i][j]` will be integers in range `[0, 99]`.

<!-- more -->

## Analysis
&emsp;&emsp;这道题考察的是矩阵（二维数组）的一些性质。从题目的角度看，我们很容易陷入一个困局，就是局限在如何提取出这么多对角线。提取出所有的对角线再逐个检查是一种效率很低的方法，我们会有更快的方法去实现。其实所谓对角线，就是**每个元素与它的右下元素比较**（若存在右下元素），我们只需要对矩阵中的每个元素都作一个检查就可以了，只要有一个不满足，则直接返回`false`。
## Solution
&emsp;&emsp;对整个二维数组进行遍历，每个元素与它的右下元素比较是否相同，注意下标越界情况即可。

## Code
```C++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        int row = matrix.size();
        int column = matrix[0].size();

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < column; j++) {
                if (i+1 < row && j+1 < column && (matrix[i][j] != matrix[i+1][j+1])) {
                    cout << i << j;
                    return false;
                }
            }
        }
        return true;
    }
};
```

## Summary
&emsp;&emsp;这道题给我很大的启发，在处理矩阵的部分问题上，不需要把特定数据提取出来，我们可以充分利用二维数组访问的便捷性，对数据进行直接的比较。本题的讲解到这里结束了，谢谢您的支持！
