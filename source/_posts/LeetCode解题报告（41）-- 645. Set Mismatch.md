---
title: LeetCode解题报告（41）-- 645. Set Mismatch
tags:
  - LeetCode
abbrlink: 22648
date: 2018-12-03 13:44:53
---
## Problem
The set `S` originally contains numbers from 1 to `n`. But unfortunately, due to the data error, one of the numbers in the set got duplicated to **another** number in the set, which results in repetition of one number and loss of another number.

Given an array `nums` representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.
<!-- more -->

**Example 1:**
```
Input: nums = [1,2,2,4]
Output: [2,3]
```

**Note:**
  1. The given array size will in the range [2, 10000].
  2. The given array's numbers won't have any order.

## Analysis
&emsp;&emsp;这道题的考点是数组操作，与前面做过的题类似，也是要找到重复的元素与丢失的元素。我们的基本思想还是不变，对每个元素，都将他放置到它应该在的位置，即`nums[i] = i+1`。遍历一遍后，找到第一个不匹配位置的元素，该元素就是重复的元素，而该元素的下标加1就是缺失元素的值。

## Solution
&emsp;&emsp;按照分析的思路coding即可，没有特别需要注意的地方。

## Code
```C++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> result;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == i+1 || nums[i] == nums[nums[i] - 1])
                continue;
            swap(nums[i], nums[nums[i] - 1]);
            i--;
        }
        for (int i = 0; i < size; i++) {
            if (nums[i] != i + 1) {
                result.emplace_back(nums[i]);
                result.emplace_back(i+1);
                break;
            }
        }
        return result;
    }
};
```

## Summary
&emsp;&emsp;这道题目帮我们复习了如果在数组中查找重复元素的方法，算法的时间复杂度是O(n)，因此使用的范围也是很广的。本题的分析到此为止，谢谢！
