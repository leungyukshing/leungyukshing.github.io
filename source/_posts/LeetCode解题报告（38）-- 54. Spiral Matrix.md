---
title: LeetCode解题报告（38）-- 54. Spiral Matrix
tags:
  - LeetCode
abbrlink: 34556
date: 2018-11-26 09:40:56
---
## Problem
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

**Example 1:**
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```
<!-- more -->

**Example 2:**
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## Analysis
&emsp;&emsp;这是一道很经典的题目——输出螺旋矩阵。难点是对矩阵遍历的技巧。一开始无从入手，因为遍历过程很容易理解，却很难用数学语言来描述。但是看多几个例子之后发现，还是有规律可循的。首先是遍历的方向，总共是四个方向：从左到右、从上到下、从右到左、从下到上，而遍历的边恰好是对应的上边、右边、下边、左边，而且这四个是按顺序不断重复的，他们之间是相互约束的关系，于是我们就可以以边为单位进行处理，四条边为一组（即一个螺旋），不断重复。那么还有一个问题就是终止条件，因为我们是以边为单位，所以如果某条边上已经无法遍历，则说明输出结束了。

## Solution
&emsp;&emsp;用一个`while(true)`来不断循环四条边的遍历，用四个变量`up`、`down`、`left`、`right`表示四条边遍历的下标，然后相互作为边界值进行便利，仅当某条边达到了边界值，则退出循环。

## Code
```C++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        int m = matrix.size();
        if (m == 0)
            return result;
        int n = matrix[0].size();
        int up = 0, down = m - 1, left = 0, right = n - 1;

        while (true) {
            // 上方，从左到右
            for (int col = left; col <= right; col++)
                result.emplace_back(matrix[up][col]);
            if (++up > down)
                break;

            // 右方，从上到下
            for (int row = up; row <= down; row++)
                result.emplace_back(matrix[row][right]);
            if (--right < left)
                break;

            // 下方，从右到左
            for (int col = right; col >= left; col--)
                result.emplace_back(matrix[down][col]);
            if (--down < up)
                break;

            // 左方，从下到上
            for (int row = down; row >= up; row--)
                result.emplace_back(matrix[row][left]);
            if (++left > right)
                break;
        }
        return result;
    }
};
```
**运行时间：**约0ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;这道题涉及到的知识点不多，但是很讲技巧。要把握好每个螺旋的规律，才可以比较合理地写出代码。本题的分析到这里，谢谢您的支持！
