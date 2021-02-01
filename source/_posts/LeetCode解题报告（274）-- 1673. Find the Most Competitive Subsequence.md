---
title: LeetCode解题报告（274）-- 1673. Find the Most Competitive Subsequence
tags:
  - LeetCode
mathjax: true
date: 2021-02-01 12:34:15

---

## Problem

Given an integer array `nums` and a positive integer `k`, return *the most **competitive** subsequence of* `nums` *of size* `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.

<!-- more -->

**Example 1:**

```
Input: nums = [3,5,2,6], k = 2
Output: [2,6]
Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
```

**Example 2:**

```
Input: nums = [2,4,3,3,5,4,9,6], k = 4
Output: [2,3,3,4]
```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `1 <= k <= nums.length`

------

## Analysis

&emsp;&emsp;这道题目给定一个字符串，要求找出最competitive的子序列。对competitive的定义是，两个字符串中，第一个不相同的字符谁小谁就更competitive。我们希望较小的字符出现在字符串靠前的位置，这个并不难实现，另外开一个字符串存放结果，如果遍历到当前的字符比结果的要小，就删掉结果，这样保证了小的字符在尽量靠前的位置。

&emsp;&emsp;但需要注意，这题还有一个长度的条件，所以我们要保证最后得到的结果是满足指定长度的，所以在删除的时候要检查长度。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        int size = nums.size();
        vector<int> result(size);
        int idx = 0;
        for (int i = 0; i < size; i++) {
            while (idx && nums[i] < result[idx - 1] && idx + size - i - 1 >= k) {
                idx--;
            }
            if (idx < k) {
                result[idx++] = nums[i];
            }
        }
        vector<int> temp(result.begin(), result.begin() + idx);
        return temp;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是字符串题目中比较常规的一类题目。这类题目通常是单独维护答案的字符串，然后通过判断逻辑去维护。这道题这道题目的分享到这里，谢谢您的支持！
