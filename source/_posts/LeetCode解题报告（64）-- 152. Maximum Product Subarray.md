---
title: LeetCode解题报告（64）-- 152. Maximum Product Subarray
tags:
  - LeetCode
abbrlink: 40760
date: 2019-09-30 01:57:14
---

## Problem

Given a (singly) linked list with head node `root`, write a function to split the Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

<!-- more -->

**Example 1:**

```
Input: 
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

------

## Analysis

&emsp;&emsp;这道题是动态规划题目的变种，要求是在一个数组中找到连续的子序列，这个子序列的乘积最大。考虑到是连续的子序列，又是求最大的问题，所以应该首先考虑动态规划。然后我们来分析**乘积最大**这个条件，乘积最大有以下几种情况：

- 最大值乘以最大值
- 最小值乘以最小值

&emsp;&emsp;因此，我们需要维护**最大值**和**最小值**。对于数组中某一个元素`a[i]`，要获得最大值，只有两种情况：

- 乘以当前最大值（当本身是正数）
- 乘以当前最小值（当本身是负数）
- 它本身

&emsp;&emsp;同样地，如果需要得到最小值，也只有两种情况：

- 乘以最大值（当本身是负数）
- 乘以最小值（当本身是正数）
- 它本身

&emsp;&emsp;我们一直维护一个当前最大值和当前最小值就可以了，最后的答案就是当前的最大值。

------

## Solution

&emsp;&emsp;计算没有太大的难度，要注意**计算最小值的时候需要用到更新前的最大值，要先保留一份副本用于计算**。

------

## Code

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int currentMax = nums[0], currentMin = nums[0];
        int size = nums.size();
        int result = currentMax;
        for (int i = 1; i < size; i++) {
            int tmp = currentMax;
            currentMax = max(max(currentMax * nums[i], currentMin * nums[i]), nums[i]);
            currentMin = min(min(tmp * nums[i], currentMin * nums[i]), nums[i]);
            
            result = max(result, currentMax);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;本题是动态规划的变种，与乘积的计算相关联，主要难点还是把握好乘积什么时候最大这个点，然后分别列出可能的情况，最好进行计算即可。这道题的分享到这里，欢迎大家评论、转发，最后，谢谢您的支持！
