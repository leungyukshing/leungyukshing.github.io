---
title: LeetCode解题报告（385) -- 1695. Maximum Erasure Value
tags:
  - LeetCode
mathjax: true
abbrlink: 6498
date: 2021-06-06 15:03:19
---

## Problem

You are given an array of positive integers `nums` and want to erase a subarray containing **unique elements**. The **score** you get by erasing the subarray is equal to the **sum** of its elements.

Return *the **maximum score** you can get by erasing **exactly one** subarray.*

An array `b` is called to be a subarray of `a` if it forms a contiguous subsequence of `a`, that is, if it is equal to `a[l],a[l+1],...,a[r]` for some `(l,r)`.

<!-- more -->

**Example 1:**

```
Input: nums = [4,2,4,5,6]
Output: 17
Explanation: The optimal subarray here is [2,4,5,6].
```

**Example 2:**

```
Input: nums = [5,2,1,2,5,2,1,2,5]
Output: 8
Explanation: The optimal subarray here is [5,2,1] or [1,2,5].
```



**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

------

## Analysis

&emsp;&emsp;这道题给出一个数组，要求在里面找到一个子数组（注意是连续的），使得数组中没有重复的元素，并且它们的和最大。

&emsp;&emsp;子序列（非连续）的题目我们一般采用dp来做，而对于子数组（连续）的题目来说我们一般使用双指针。一个指针指向子数组的起点，另一个指向终点。首先不断移动右指针去扩大范围，以获得更大的和。然后每次扩大范围后，需要检查数组中是否有元素重复，这里借用了set去做去重。如果扩大右指针后发现子数组中有重复的元素，此时应该从左指针处开始移除元素，直至保证没有重复的元素。

&emsp;&emsp;在增、删数字时，同时更新`sum `，表示当前子数组元素之和，同时全局维护一个极大值即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int maximumUniqueSubarray(vector<int>& nums) {
        set<int> s;
        int i = 0, j = 0, size = nums.size();
        int sum = 0, result = 0;
        
        while (j < size) {
            if (s.count(nums[j])) {
                sum -= nums[i];
                s.erase(nums[i]);
                i++;
            } else {
                sum += nums[j];
                s.insert(nums[j]);
                j++;
                result = max(result, sum);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目还算是比较典型的子数组题目，依靠两个指针去定位到数组。同时还使用了set去做去重。这道题目的分享到这里，感谢你的支持！
