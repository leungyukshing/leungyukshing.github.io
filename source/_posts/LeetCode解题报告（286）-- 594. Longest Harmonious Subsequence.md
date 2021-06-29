---
title: LeetCode解题报告（286）-- 594. Longest Harmonious Subsequence
tags:
  - LeetCode
mathjax: true
abbrlink: 18248
date: 2021-02-09 20:54:29
---

## Problem

We define a harmonious array as an array where the difference between its maximum value and its minimum value is **exactly** `1`.

Given an integer array `nums`, return *the length of its longest harmonious subsequence among all its possible subsequences*.

A **subsequence** of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

<!-- more -->

**Example 1:**

```
Input: nums = [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: 2
```

**Example 3:**

```
Input: nums = [1,1,1,1]
Output: 0
```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `-109 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求找出最长的子数组，这个**数组的极差恰好为1**。极差恰好为1，说明这个数组只能包含两种元素，且他们的差是1。而且题目要求我们返回的只是长度，而不需要实际的数组，所以求解过程中我们关注长度就可以了。

&emsp;&emsp;先遍历一遍，把每个数字出现的次数都统计起来，然后逐个数字进行判断。对`nums[i]`，先判断`nums[i + 1]`是否存在。如果不存在则说明`nums[i]`肯定不可能是组成答案的其中一个数字；如果存在，则说明能构成答案，这个时候长度就是两种数字出现的和。整个遍历过程我们维护一个最大值即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int, int> m;
        for (auto num: nums) {
            m[num]++;
        }
        
        int result = 0;
        for (auto a: m) {
            if (m.count(a.first + 1)) {
                result = max(result, m[a.first] + m[a.first + 1]);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
