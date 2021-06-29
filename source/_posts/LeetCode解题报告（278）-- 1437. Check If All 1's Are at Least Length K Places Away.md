---
title: LeetCode解题报告（278）-- 1437. Check If All 1's Are at Least Length K Places Away
tags:
  - LeetCode
mathjax: true
abbrlink: 20985
date: 2021-02-03 22:21:36
---

## Problem

Given an array `nums` of 0s and 1s and an integer `k`, return `True` if all 1's are at least `k` places away from each other, otherwise return `False`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/04/15/sample_1_1791.png)

```
Input: nums = [1,0,0,0,1,0,0,1], k = 2
Output: true
Explanation: Each of the 1s are at least 2 places away from each other.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/04/15/sample_2_1791.png)

```
Input: nums = [1,0,0,1,0,1], k = 2
Output: false
Explanation: The second 1 and third 1 are only one apart from each other.
```

**Example 3:**

```
Input: nums = [1,1,1,1,1], k = 0
Output: true
```

**Example 4:**

```
Input: nums = [0,1,0,1], k = 1
Output: true
```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= k <= nums.length`
- `nums[i]` is `0` or `1`

------

## Analysis

&emsp;&emsp;题目给出一个只包含0和1的数组，还有一个间隔`k`，问这个数组中相邻的1之间的距离是否都至少为`k`。思路很简单，直接一遍遍历，用一个下标记录上一次出现1的位置，然后找到1，下标做差就能计算出之间的距离。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int idx = -1;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (idx == -1 && nums[i] == 1) {
                idx = i;
            } else if (nums[i] == 1) {
                if (i - idx <= k) {
                    return false;
                } else {
                    idx = i;
                }
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道非常简单的数组类题目，仅在这里做一下总结，没有过多的算法知识分享。这道题这道题目的分享到这里，谢谢您的支持！
