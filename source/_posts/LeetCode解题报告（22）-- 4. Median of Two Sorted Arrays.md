---
title: LeetCode解题报告（22）-- 4. Median of Two Sorted Arrays
tags:
  - LeetCode
abbrlink: 46686
date: 2018-10-22 09:36:18
---
## Problem
There are two sorted arrays **nums1** and **nums2** of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume **nums1** and **nums2** cannot be both empty.
<!-- more -->

**Example 1**:
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

**Example 2**:
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```
## Analysis
&emsp;&emsp;这道题是有关算法的题目，要求在两个有序的数组中找到他们的中位数。第一感觉是将两个数组合并，然后找中位数，这种做法很简单，但是复杂度就比较高了。我们能不能利用原有的两个数组是有序的这个性质，来降低算法复杂度呢？答案当然是可以的。在探讨这个问题之前，我们先来探讨**求第k大数问题**。因为我们要求的中位值就是第`(m + n + 1)`大的数。
&emsp;&emsp;对于两个有序的数组，我们找第k大数，可以转化成对k的二分，目的是**每次减少其中一个数组的一半元素**。对于一般情况，我们令：
```C++
int midValue1 = A[abegin + k / 2 - 1];
int midValue2 = B[bbegin + k / 2 - 1];
```
&emsp;&emsp;其中`abegin`和`bbegin`分别标志在A中的搜索起点和在B中的搜索起点。
&emsp;&emsp;然后我们就可以比较`midValue1`和`midValue2`，分两种情况讨论：
  + `midValue1 < midValue2`，说明A数组中从`abegin`到`abegin + k / 2 - 1`的元素都不会是第k大的，因此可以排除掉。
  + `midValue1 >= midValue2`，同理，可以排除掉B数组中从`bbegin`到`bbegin + k / 2 - 1`的元素。

&emsp;&emsp;上面的排除规则是基于`A`、`B`两个数组都是有序的，因此可以很轻易地排除元素。对于边界情况，我们需要特别注意：
  + 如果`abegin`越界，说明`A`已经搜索完并全部被排除，则直接找`B`中第k个元素就可以了。`bbegin`越界也是一样的处理。
  + 如果`k = 1`，则说明要找最小的元素，只需要比较两个数组第一个元素中较小的一个就可以了。注意这里说的第一个是指`A[abegin]`和`B[bbegin]`。

## Solution
&emsp;&emsp;按照上面给出的算法递归处理就可以了，这里重点分析越界情况。越界了说明这个数组中没有要二分的情况，因此设置为`INT_MAX`，使得函数去二分另一个数组。同时，我们也无需担心同时越界的情况会发生，因为两个数组不可能同时为空，因此中位数是总是存在的。
&emsp;&emsp;在应用`findKTh()`时，我们是否需要关心两个数组的数量是基数还是偶数呢？答案是不需要，这里给出的是通用的计算通式，使用`(m + n + 1) / 2`和`(m + n + 2) / 2`计算即可。举个例子：
  + 如果m = 4, n = 5，我们计算的是第5大和第5大的数的平均数，结果还是第5大的数；
  + 如果m = 4, n = 6，我们计算的是第5大和第6大的数的平均数，符合中位数求解要求。

## Code
```C++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        return ((findKTh(nums1, 0, nums2, 0, (m + n + 1 ) / 2) + findKTh(nums1,0, nums2, 0, (m + n + 2) / 2)) / 2.0);
    }
private:
    int findKTh(vector<int> A, int abegin, vector<int> B, int bbegin, int k) {
        if (abegin >= A.size()) {
            return B[bbegin + k - 1];
        }
        if (bbegin >= B.size()) {
            return A[abegin + k - 1];
        }
        if (k == 1) {
            return min(A[abegin], B[bbegin]);
        }
        int midValue1 = INT_MAX, midValue2 = INT_MAX;
        if (abegin + k / 2 - 1 < A.size()) {
            midValue1 = A[abegin + k / 2 - 1];
        }
        if (bbegin + k / 2 - 1 < B.size()) {
            midValue2 = B[bbegin + k / 2 - 1];
        }
        if (midValue1 < midValue2) {
            return findKTh(A, abegin + k / 2, B, bbegin, k - k / 2);
        }
        else {
            return findKTh(A, abegin, B, bbegin + k / 2, k - k / 2);
        }
    }
};
```
**运行时间：**约36ms，超过74.53%的CPP代码。

## Summary
&emsp;&emsp;这道题我认为非常有价值，它的本质是对**找第k大数**算法的实现，而这个算法也是老师在课上讲到的。它的巧妙之处在于充分利用两个数组各自有序的特点，对k进行二分，不断地选择一个数组减少k/2个元素，直至找到第k大数。对这道题的分析到这里了，谢谢您的支持！
