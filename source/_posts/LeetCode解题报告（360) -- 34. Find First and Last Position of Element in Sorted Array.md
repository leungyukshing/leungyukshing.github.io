---
title: LeetCode解题报告（360) -- 34. Find First and Last Position of Element in Sorted Array
tags:
  - LeetCode
mathjax: true
date: 2021-05-02 12:19:11

---

## Problem

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

**Follow up:** Could you write an algorithm with `O(log n)` runtime complexity?

<!-- more -->

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个有序数组，还有一个`target`，要求找出target在数组中第一次和最后一次出现的下标。其实题目非常简单了，因为数组是有序的，所以也保证了相同的值是靠在一起的，从左往右开始遍历，我们只需要在第一次出现的时候记录下一个下标，然后最后一次出现的时候记录下标，这就是答案了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int size = nums.size();
        vector<int> result;
        int length = 0;
        bool flag = true;
        for (int i = 0; i < size; i++) {
            if (nums[i] == target) {
                if (flag) {
                    flag = false;
                    result.push_back(i);                    
                }
                length++;
            }
        }
        if (flag) {
            result.push_back(-1);
            result.push_back(-1);
        } else {
            result.push_back(result[0] + length - 1);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
