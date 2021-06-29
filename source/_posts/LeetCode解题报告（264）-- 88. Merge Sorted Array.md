---
title: LeetCode解题报告（264）-- 88. Merge Sorted Array
tags:
  - LeetCode
mathjax: true
abbrlink: 28980
date: 2021-01-12 21:20:14
---

## Problem

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into `nums1` as one sorted array.

The number of elements initialized in `nums1` and `nums2` are `m` and `n` respectively. You may assume that `nums1` has enough space (size that is equal to `m + n`) to hold additional elements from `nums2`.

<!-- more -->

**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

**Example 2:**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
```

**Constraints:**

- `0 <= n, m <= 200`
- `1 <= n + m <= 200`
- `nums1.length == m + n`
- `nums2.length == n`
- `-109 <= nums1[i], nums2[i] <= 109`

------

## Analysis

&emsp;&emsp;这道题目是把两个有序的数组，其实也就是归并排序中merge的步骤。题目给出了`nums1`和`nums2`，`nums1`也同时用于最后存放结果，所以是足够大的。这也说明`nums1`后面的位置是空的，所以我们从后往前进行赋值，这样就能减少元素的交换。

------

## Solution

&emsp;&emsp;用三个下标`i`、`j`、`k`分别指向`nums1`（从m-1开始）、`nums2`和`nums1`从（m+n-1开始），比较`nums1[i]`和`nums2[j]`，对`nums[k]`赋值。这里有个tricky的地方，当两两对比完成后，我们需要处理剩下的。如果剩下`nums2`，就逐一赋值；如果剩下的是`nums1`则不需要，因为元素本身就在里面了。

------

## Code

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] <= nums2[j]) {
                nums1[k] = nums2[j];
                k--;
                j--;
            } else {
                nums1[k] = nums1[i];
                k--;
                i--;
            }
        }
        while (j >= 0) {
            nums1[k] = nums2[j];
            j--;
            k--;
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是非常简单的有序数组合并，但是要快速求解仍然需要观察。利用`nums1`后面的位置是空的这个特点，从后往前进行。这道题这道题目的分享到这里，谢谢您的支持！
