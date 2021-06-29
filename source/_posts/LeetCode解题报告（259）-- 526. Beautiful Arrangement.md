---
title: LeetCode解题报告（259）-- 526. Beautiful Arrangement
tags:
  - LeetCode
mathjax: true
abbrlink: 13881
date: 2021-01-05 01:56:30
---

## Problem

Suppose you have `n` integers labeled `1` through `n`. A permutation of those `n` integers `perm` (**1-indexed**) is considered a **beautiful arrangement** if for every `i` (`1 <= i <= n`), **either** of the following is true:

- `perm[i]` is divisible by `i`.
- `i` is divisible by `perm[i]`.

Given an integer `n`, return *the **number** of the **beautiful arrangements** that you can construct*.

<!-- more -->

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: 
The first beautiful arrangement is [1,2]:
    - perm[1] = 1 is divisible by i = 1
    - perm[2] = 2 is divisible by i = 2
The second beautiful arrangement is [2,1]:
    - perm[1] = 2 is divisible by i = 1
    - i = 2 is divisible by perm[2] = 1
```

**Example 2:**

```
Input: n = 1
Output: 1
```

**Constraints:**

- `1 <= n <= 15`

------

## Analysis

&emsp;&emsp;这道题目给定一个`N`，要求使用`[1, N]`这些数字去组成一个数组，要求是第`i`个位置的值能够整除`i`或者`i`能够整除第`i`个位置，最后要求出这些满足要求的数组一共有多少个。

&emsp;&emsp;所以最直接的思路就是求出所有的全排列数组，然后在其中挑选出满足要求的组合。生成全排列直接使用[递归的方法](https://blog.csdn.net/K346K346/article/details/51154786)即可，然后每个位置都检查一下是否满足条件即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int countArrangement(int n) {
        vector<int> nums(n);
        for (int i  = 0; i < n; i++) {
            nums[i] = i + 1;
        }
        return helper(n, nums);
    }
private:
    int helper(int n, vector<int>& nums) {
        if (n <= 0) {
            return 1;
        }
        int result = 0;
        for (int i = 0; i < n; i++) {
            if (n % nums[i] == 0 || nums[i] % n == 0) {
                swap(nums[i], nums[n - 1]);
                result += helper(n - 1, nums);
                swap(nums[i], nums[n - 1]);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目最值得总结的地方是递归生成全排列，全排列在数组类题目中是非常重要的，使用递归生成coding非常简单，而且嵌入逻辑也很方便，所以很有必要总结。这道题这道题目的分享到这里，谢谢您的支持！
