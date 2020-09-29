---
title: LeetCode解题报告（173）-- 713. Subarray Product Less Than K
tags:
  - LeetCode
mathjax: true
date: 2020-09-30 00:44:21


---

## Problem

Your are given an array of positive integers `nums`.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than `k`.

<!-- more -->

**Example 1:**

```
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Note:**

`0 < nums.length <= 50000`.

`0 < nums[i] < 1000`.

`0 <= k < 10^6`.

------

## Analysis

&emsp;&emsp;这道题用暴力搜索求解会超时，所以我们要减少时间复杂度。因为是子序列，而且数组中所有的数字都是正整数，这也就意味着区间越大，乘积越大，所以可以采用滑动窗口的机制。

&emsp;&emsp;所谓滑动窗口，就是不断地增加`right`，然后判断乘积是否小于`k`。如果小于则可以继续往右增加，如果大于则需要移动`left`了。这里我们就知道了怎么滑动这个窗口了，但是怎么根据这个窗口去计算出子数组的数量呢？

&emsp;&emsp;实际上这个窗口的大小，就是包含`right`的子数组个数。举个例子，如`[10, 5, 2]`的长度是3，则这个窗口就包含了`[5, 2]`，`[2]`这两种情况了。

------

## Solution

&emsp;&emsp;求解过程没有用到特殊的数据结构，需要注意计算子数组长度的时候要稍微注意即可。

------

## Code

```c++
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int size = nums.size(), result = 0;
        int total = 1;
        int start = 0;
        for (int i = 0; i < size; i++) {
                total *= nums[i];
                while (start <= i && total >= k) {
                    total /= nums[start];
                    start++;
                }
                result += i - start + 1;
        }
        return result;        
    }
};
```

------

## Summary

&emsp;&emsp;这道题是不用dp做的一道子数组的题目，原因是他求的是子数组的个数，所以这类题目之后都可以参考这里的滑动窗口做法。这道题的分析到这里，谢谢！
