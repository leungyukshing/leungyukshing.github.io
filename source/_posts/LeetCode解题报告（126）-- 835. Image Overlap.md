---
title: LeetCode解题报告（126）-- 835. Image Overlap
tags:
  - LeetCode
mathjax: true
abbrlink: 48018
date: 2020-09-06 15:23:40
---

## Problem

Two images `A` and `B` are given, represented as binary, square matrices of the same size.  (A binary matrix has only 0s and 1s as values.)

We translate one image however we choose (sliding it left, right, up, or down any number of units), and place it on top of the other image.  After, the *overlap* of this translation is the number of positions that have a 1 in both images.

(Note also that a translation does **not** include any kind of rotation.)

What is the largest possible overlap?

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/12/18/q2-e1.png)

```
Input: A = [[1,1,0],
            [0,1,0],
            [0,1,0]]
       B = [[0,0,0],
            [0,1,1],
            [0,0,1]]
Output: 3
Explanation: We slide A to right by 1 unit and down by 1 unit.
```

**Notes:** 

1. `1 <= A.length = A[0].length = B.length = B[0].length <= 30`
2. `0 <= A[i][j], B[i][j] <= 1`

------

## Analysis

&emsp;&emsp;这道题题目给出了两个矩阵，代表两个图片，其中的值是二值。允许的操作是滚动，包括上下滚动和左右滚动。题目要求我们通过若干次滚动操作，得到值为1的位置重复的最多的个数。

&emsp;&emsp;之前很多篇题解也提到过，这类操作类型的题目最直接的解法就是穷举。矩阵的size是知道的，所以一张图片反转的可能结果就是`n * n`，然后两个图片的结果相互比对，找到最重复最多的就可以了。

&emsp;&emsp;当然了，我写题解肯定要有比暴力搜索更好的方法。从上面的方法中可以看出来，实际上我们要的只是重叠最多的这个结果，反转操作的中间结果耗费了很多时间和内存，这些并不是我们需要的最终结果，它们只是用来帮助判断的。所以我们可以把滚动操作的这个步骤给简化。

&emsp;&emsp;在A中的某个1，要移动到B中的某个1，它必定是要进行横向和纵向的移动的。横向移动的距离是它们直接的横坐标之差，纵向移动的距离是它们的纵坐标之差。如果拥有相同的横向移动坐标和相同的纵向移动坐标，则说明这两个点它们经历的移动的操作是一样的，所以它们会在同一批重复，这样我们只需要找到移动操作相同的最多的数量，就是答案了。

------

## Solution

&emsp;&emsp;为了记录移动操作同时对比是否一致，我们可以把横坐标移动的距离和纵坐标移动的距离组成一个字符串在识别，比如横坐标移动3，纵坐标移动2，那么字符串就是`3-2`。

------

## Code

```c++
class Solution {
public:
    int largestOverlap(vector<vector<int>>& A, vector<vector<int>>& B) {
        vector<vector<int>> onesA, onesB;
        unordered_map<string, int> diff;
        
        int n = A.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (A[i][j]) {
                    onesA.push_back({i, j});
                }
                if (B[i][j]) {
                    onesB.push_back({i, j});
                }
            }
        }
        
        int size1 = onesA.size();
        int size2 = onesB.size();
        
        for (int i = 0; i < size1; i++) {
            for (int j = 0; j < size2; j++) {
                diff[to_string(onesA[i][0] - onesB[j][0]) + "-" + to_string(onesA[i][1] - onesB[j][1])]++;
            }
        }
        
        int result = 0;
        for (auto d: diff) {
            result = max(result, d.second);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的关键就是把握好移动的本质，实际上是横坐标和纵坐标的改变。这道题的分析到这里，谢谢！
