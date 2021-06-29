---
title: LeetCode解题报告（306)-- 645. Set Mismatch
tags:
  - LeetCode
mathjax: true
abbrlink: 13190
date: 2021-03-05 01:23:44
---

## Problem

You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in **repetition of one** number and **loss of another** number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return *them in the form of an array*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,2,4]
Output: [2,3]
```

**Example 2:**

```
Input: nums = [1,1]
Output: [1,2]
```

**Constraints:**

- `2 <= nums.length <= 104`
- `1 <= nums[i] <= 104`

------

## Analysis

&emsp;&emsp;题目给出了一个数组，里面包含`[1, n]`的数字，其中有一个数字会出现两次，因此肯定会有一个数字缺失，题目要求我们找出重复出现的数字和缺失的数字。这类题目的常用思路前面也有分享过，对于在长度为`n`的数组中包含`[1,n]`的数字，有一个很重要的特点就是可以用值本身去作为下标。这里的思路也是一样的，把值-1当成下标去访问，然后把该位置上的数字更新为相反数，这样做的好处就是，如果这个某个值更新前已经是负数，说明这个值就是重复的值。更新完一遍后，找出值为正数的下标，这个说明这个值是重复的，所以这个下标对应的值就是缺失了，此时用下标把缺失值计算出来即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int size = nums.size();
        vector<int> result;
        for (int i = 0; i < size; i++) {
            if (nums[abs(nums[i]) - 1] < 0) {
                result.push_back(abs(nums[i]));
            } else {
                nums[abs(nums[i]) - 1] *= -1;
            }
        }
        
        for (int i = 0; i < size; i++) {
            if (nums[i] > 0) {
                result.push_back(i + 1);
                break;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目一是利用了值作为下标的数组特性，而是巧用了相反数来求解。可以总结出，在处理数字重复问题时，除了用map去统计，使用相反数也是一个很好的方向。这道题目的分享到这里，感谢你的支持！
