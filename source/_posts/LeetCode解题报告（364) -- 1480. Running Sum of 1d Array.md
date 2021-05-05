---
title: LeetCode解题报告（364) -- 1480. Running Sum of 1d Array
tags:
  - LeetCode
mathjax: true
date: 2021-05-05 15:04:32

---

## Problem

Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.

Return the running sum of `nums`.

You will start on the `1st` day and you cannot take two or more courses simultaneously.

Return *the maximum number of courses that you can take*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```

**Example 2:**

```
Input: nums = [1,1,1,1,1]
Output: [1,2,3,4,5]
Explanation: Running sum is obtained as follows: [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1].
```

**Example 3:**

```
Input: nums = [3,1,2,10,1]
Output: [3,4,6,16,17]
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `-10^6 <= nums[i] <= 10^6`

------

## Analysis

&emsp;&emsp;这道题目就是一维数组简单的presum了，从头一直往后加就行。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        int size = nums.size();
        vector<int> result(size, 0);
        result[0] = nums[0];
        for (int i = 1; i < size; i++) {
            result[i] = result[i - 1] + nums[i];
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
