---
title: LeetCode解题报告（307)-- 268. Missing Number
tags:
  - LeetCode
mathjax: true
date: 2021-03-06 02:26:28

---

## Problem

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*

**Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

<!-- more -->

**Example 1:**

```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2:**

```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 3:**

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

**Example 4:**

```
Input: nums = [0]
Output: 1
Explanation: n = 1 since there is 1 number, so all numbers are in the range [0,1]. 1 is the missing number in the range since it does not appear in nums.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- All the numbers of `nums` are **unique**.

------

## Analysis

&emsp;&emsp;这道题目和上一题有点类似，但是有一点不同，这里是长度为`n`的数组，但是数字是`[0,n]`一共`n+1`个，所以没办法与下标一一对应。但是这里只需要求出缺失的值，所以很简单的思路就是把前n项求和，减去数组中的元素，剩下的结果就是缺失的数字了。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int sum = 0;
        int n = nums.size();
        for (auto &num: nums) {
            sum += num;
        }
        return 0.5 * n * (n + 1) - sum;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上和数组并没有很大关系，巧用数学公式来求解即可。这道题目的分享到这里，感谢你的支持！
