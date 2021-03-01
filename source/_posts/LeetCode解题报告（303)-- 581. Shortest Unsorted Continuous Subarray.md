---
title: LeetCode解题报告（303)-- 581. Shortest Unsorted Continuous Subarray
tags:
  - LeetCode
mathjax: true
date: 2021-03-02 00:33:10

---

## Problem

Given an integer array `nums`, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order.

Return *the shortest such subarray and output its length*.

<!-- more -->

**Example 1:**

```
Input: nums = [2,6,4,8,10,9,15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: 0
```

**Example 3:**

```
Input: nums = [1]
Output: 0
```

**Constraints:**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`

 

**Follow up:** Can you solve it in  time complexity?

------

## Analysis

&emsp;&emsp;题目给出了一个数组，要求找长度最短的子数组，使得对这个子数组进行排序后，整个数组变得有序。其实这里的想法也非常简单，我们从左往右进行遍历，首先找到第一次不满足有序条件的数，假设是`nums[i] < nums[i - 1]`，然后向前进行交换，直至`nums[i]`交换到它应该在的位置，此时这个位置就是子数组的起点，而原来的`i`就是子数组的终点，这样我们就能得到一个结果。遍历过程中会有很多个区间的长度，最终维护一个极大值即可。

------

## Solution

&emsp;&emsp;确定起点需要考虑更多的细节，比如还没开始前，起点`start`默认是-1。然后每次找到新的起点后，需要和原来的`start`做对比，只有当新的起点在`start`的左边才更新。

------

## Code

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int res = 0, start = -1, n = nums.size();
        for (int i = 1; i < n; ++i) {
            if (nums[i] < nums[i - 1]) {
                int j = i;
                while (j > 0 && nums[j] < nums[j - 1]) {
                    swap(nums[j], nums[j - 1]);
                    --j;
                }
                if (start == -1 || start > j) start = j;
                res = i - start + 1;
            }
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道难度不大的数组类题目，主要是计算区间的长度，一般这类题目都可以使用两个循环进行求解，全局维护一个极值即可。这道题目的分享到这里，感谢你的支持！
