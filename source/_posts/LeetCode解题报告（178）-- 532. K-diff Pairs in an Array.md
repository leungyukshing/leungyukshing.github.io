---
title: LeetCode解题报告（178）-- 532. K-diff Pairs in an Array
tags:
  - LeetCode
mathjax: true
abbrlink: 61208
date: 2020-10-04 01:00:24
---

## Problem

Given an array of integers `nums` and an integer `k`, return *the number of **unique** k-diff pairs in the array*.

A **k-diff** pair is an integer pair `(nums[i], nums[j])`, where the following are true:

- `0 <= i, j < nums.length`
- `i != j`
- `a <= b`
- `b - a == k`

<!-- more -->

**Example 1:**

```
Input: nums = [3,1,4,1,5], k = 2
Output: 2
Explanation: There are two 2-diff pairs in the array, (1, 3) and (3, 5).
Although we have two 1s in the input, we should only return the number of unique pairs.
```

**Example 2:**

```
Input: nums = [1,2,3,4,5], k = 1
Output: 4
Explanation: There are four 1-diff pairs in the array, (1, 2), (2, 3), (3, 4) and (4, 5).
```

**Example 3:**

```
Input: nums = [1,3,1,5,4], k = 0
Output: 1
Explanation: There is one 0-diff pair in the array, (1, 1).
```

**Example 4:**

```
Input: nums = [1,2,4,4,3,3,0,9,2,3], k = 3
Output: 2
```

**Example 5:**

```
Input: nums = [-1,-2,-3], k = 1
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 104`
- `-107 <= nums[i] <= 107`
- `0 <= k <= 107`

------

## Analysis

&emsp;&emsp;这道题目要在给定的数组中找到差为`k`的数对，其实有点像是two sum的变种。假设数组中存在两个数`a`、`b`，使得`a - b = k`。如果在遍历到`a`的时候，我们只需要在数组中找一下是否存在`a - k`的数，如果存在，则说明这个是一个答案。这样下去就能够找到所有符合要求的答案了。

------

## Solution

&emsp;&emsp;数据结构方面原来使用set和map是差不多的，因为绝大多数情况下我们只需要检查有没有这个数，而不考虑它出现的次数。但是有一种edge case需要考虑，就是当`k`为0的时候，两个相同的数是满足答案的要求的，所以要选用map，这样就是记录数字出现的次数了。

------

## Code

```c++
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        unordered_map<int, int> m;
        for (auto num: nums) {
            m[num]++;
        }
        
        int result = 0;
        int size = nums.size();
        for (auto temp: m) {
            if (k == 0 && temp.second > 1) {
                result++;
            } else if (k != 0 && m.count(temp.first - k)) {
                result++;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;想这类在数组中找一对的题目，可以优先考虑使用map，因为一旦确定一个元素之后，另一个元素也是可以确定的。这道题的分析到这里，谢谢！
