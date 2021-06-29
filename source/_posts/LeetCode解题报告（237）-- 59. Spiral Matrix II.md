---
title: LeetCode解题报告（237）-- 59. Spiral Matrix II
tags:
  - LeetCode
mathjax: true
abbrlink: 62165
date: 2020-12-07 20:40:52
---

## Problem

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```

**Constraints:**

- `1 <= n <= 20`

------

## Analysis

&emsp;&emsp;这道题是旋转矩阵的系列题，要求我们生成一个旋转矩阵。矩阵的生成顺序没多少文章可以做。观察纵向、横向，好像都没什么规律可言，因为矩阵本身最大的规律就是旋转生成，所以我们就要用代码来实现这个旋转。

&emsp;&emsp;这个旋转可以分为两个部分，第一个是旋转了多少圈，第二个是四个方向（从左到右、从上到下、从右到左、从下到上）的走法。旋转多少圈是比较容易计算的，因为一圈肯定是上半部分和下半部分各走一半，所以圈数就是边长的一半，即$\frac{n}{2}$（暂不讨论`n`奇偶的情况）。然后就看四个方向的走法，可以看到，除去以开水从左到右外，其余走的数量都是一样的，而且走的数量和圈数也是有关系的。圈数增加一圈，走的长度就会减少一半。找到这个规律后，我们就可以用圈数来限定我们移动的距离，当移动的距离够的时候，就切换方向。

&emsp;&emsp;这里还有`n`的奇偶的情况要特殊讨论。当`n`为偶数的时候，圈数就是刚刚好的整数，直接计算就可以了；但是当`n`为奇数的时候，中间就会多出来一个位置`res[n / 2][n / 2]`，这里是不满足一圈的，所以在这种情况下，我们最后还要填回去。

------

## Solution

&emsp;&emsp;coding方面没有特别的难点，主要就是控制四个方向的下标即可。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n));
        int value = 1, p = n;
        
        for (int i = 0; i < n / 2; i++, p -= 2) {
            for (int col = i; col < i + p; col++) {
                result[i][col] = value++;
            }
            for (int row = i + 1; row < i + p; row++) {
                result[row][i + p - 1] = value++;
            }
            for (int col = i + p -2; col >= i; col--) {
                result[i + p - 1][col] = value++;
            }
            for (int row = i + p - 2; row > i; row--) {
                result[row][i] = value++;
            }
        }
        
        // central element
        if (n & 2 != 0) {
            result[n / 2][n / 2] = value;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;旋转矩阵是比较常见的面试题，它的难度不大但是很考察找规律分析的能力，以及对数组遍历的应用。这道题目的分享到这里，谢谢您的支持！
