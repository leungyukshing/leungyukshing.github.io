---
title: LeetCode解题报告（249）-- 910. Smallest Range II
tags:
  - LeetCode
mathjax: true
abbrlink: 22411
date: 2020-12-21 22:32:59
---

## Problem

Given an array `A` of integers, for each integer `A[i]` we need to choose **either x = -K or x = K**, and add `x` to `A[i] **(only once)**`.

After this process, we have some array `B`.

Return the smallest possible difference between the maximum value of `B` and the minimum value of `B`.

<!-- more -->

**Example 1:**

```
Input: A = [1], K = 0
Output: 0
Explanation: B = [1]
```

**Example 2:**

```
Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]
```

**Example 3:**

```
Input: A = [1,3,6], K = 3
Output: 3
Explanation: B = [4,6,3]
```

**Note:**

1. `1 <= A.length <= 10000`
2. `0 <= A[i] <= 10000`
3. `0 <= K <= 10000`

------

## Analysis

&emsp;&emsp;这道题目是Smallest Range的系列题，第一道系列题比较简单，因为可以加减`[-K, K]`中的任何一个数字，所以只需要最小值加上`K`，最大值减去`-K`，两者再作差和0取最小值，就能找到最小的range。但是这道题目就比较复杂，因为只能加减两个数，所以数组中任何一个数字加减后都有可能成为新的最大值（最小）。

&emsp;&emsp;为了让差值较小，我们希望用一个较小的数字加上`K`，用一个较大的数字减去`K`。我们先对数组进行排序，然后在`i`位置把数组一分为二，前面较小的加上`K`，后面较大的减去`K`。在前半部分`[0, i]`中，最大值是`A[i] + K`，最小值是`A[0] + K`；在后半部分`[i + 1, size - 1]`中，最大值是`A[size - 1] - K`，最小值是`A[i + 1] - K`，所以整个数组的最大值是`max(A[i] + K, A[size - 1] - K)`，最小值是`min(A[0] + K, A[i + 1] - K)`。因为`A[0] + K`和`A[size - 1] - K`是确定的，所以我们只需要遍历`i`，每次都计算出range，然后记录下最小的一个即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int smallestRangeII(vector<int>& A, int K) {
        sort(A.begin(), A.end());
        int size = A.size();
        int result = A[size - 1] - A[0];
        
        int leftMinValue = A[0] + K;
        int rightMaxValue = A[size - 1] - K;
        
        for (int i = 0; i < size - 1; i++) {
            int high = max(rightMaxValue, A[i] + K);
            int low = min(leftMinValue, A[i + 1] - K);
            result = min(result, high - low);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是数组类型中计算range的题目，本质上是一个确定最大最小值的问题，这道题目的技巧是把数组用`i`分开，然后讨论加上K和减去K的情况。这道题目的分享到这里，谢谢您的支持！
