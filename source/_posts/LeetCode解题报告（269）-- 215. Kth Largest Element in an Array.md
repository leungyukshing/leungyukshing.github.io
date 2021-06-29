---
title: LeetCode解题报告（269）-- 215. Kth Largest Element in an Array
tags:
  - LeetCode
mathjax: true
abbrlink: 60780
date: 2021-01-17 01:24:42
---

## Problem

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

<!-- more -->

**Example 1:**

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:**
You may assume k is always valid, 1 ≤ k ≤ array's length.

------

## Analysis

&emsp;&emsp;题目要求找出一个数组中第`K`大的数字，很明显这个就是最大堆的使用场景。我们用题目给出的数组构建一个最大堆，然后不断移除堆顶，一共移除`K-1`个，此时的堆顶元素就是第`K`大了。

------

## Solution

&emsp;&emsp;C++的STL给我们提供了优先队列，可以理解为是最大堆，所以直接使用即可。

------

## Code

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> q;
        int size = nums.size();
        
        for (int i = 0; i < size; i++) {
            q.push(nums[i]);
        }
        
        k--;
        while (k--) {
            q.pop();
        }
        return q.top();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是最大堆（优先队列）使用的经典例子，非常值得总结。这道题这道题目的分享到这里，谢谢您的支持！
