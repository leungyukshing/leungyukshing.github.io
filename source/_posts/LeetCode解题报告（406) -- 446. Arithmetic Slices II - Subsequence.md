---
title: LeetCode解题报告（406) -- 446. Arithmetic Slices II - Subsequence
mathjax: true
abbrlink: 32626
date: 2021-09-10 17:01:45
---

## Problem

Given an integer array `nums`, return *the number of all the **arithmetic subsequences** of* `nums`.

A sequence of numbers is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

- For example, `[1, 3, 5, 7, 9]`, `[7, 7, 7, 7]`, and `[3, -1, -5, -9]` are arithmetic sequences.
- For example, `[1, 1, 2, 5, 7]` is not an arithmetic sequence.

A **subsequence** of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

- For example, `[2,5,10]` is a subsequence of `[1,2,1,**2**,4,1,**5**,**10**]`.

The test cases are generated so that the answer fits in **32-bit** integer.

<!-- more -->

**Example 1:**

```
Input: nums = [2,4,6,8,10]
Output: 7
Explanation: All arithmetic subsequence slices are:
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]
```

**Example 2:**

```
Input: nums = [7,7,7,7,7]
Output: 16
Explanation: Any subsequence of this array is arithmetic.
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `-231 <= nums[i] <= 231 - 1`

------

## Analysis

&emsp;&emsp;题目给出一个数组`nums`，要求找出里面所有的子等差数列。因为数组中的数字不一定是连续的，即可以从`nums`中挑取一些数字出来组成新的等差数列，所以这里优先考虑使用dp。如果把`dp[i]`定义为到第`i`个位置子等差数列的数量，虽然比较直观，但是并不方便计算。因为在处理`dp[i]`的时候，即使找到了与`dp[j]（j <= i）`的差，也很难直接更新，因为我们不知道与其他数据的关系，所以更加很难计算等差数列的数量。

&emsp;&emsp;因此要调整一下`dp[i]`的内容。对于等差数列来说，最重要的就是等差，我们要记录好这个差。只要我们知道前面不同的差一共有多少个数字，我们就能计算出等差数列的个数。比如在处理`[2,4,6]`的时候，我们处理4，就能知道差为2的有1个；在处理6的时候，先碰到2，计算出差为4，所以是1个，然后再碰到4，计算出差为2，同时4本身记录着差为2的有1个，加入到结果中，同时更新`dp[2]`为2，以此类推。

&emsp;&emsp;所以经过上面的分析，`dp[i]`的内容应该是一个hashmap，key是差，value是出现的次数。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
        int size = nums.size();
        int result = 0;
        vector<map<int, int>> dp(size);
    
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < i; j++) {
                long diff = (long)nums[i] - nums[j];
                if (diff > INT_MAX || diff < INT_MIN) {
                    continue;
                }
                
                int di = (int)diff;
                dp[i][diff]++;
                if (dp[j].count(diff)) {
                    result += dp[j][diff];
                    dp[i][diff] += dp[j][diff];
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是难度比较大的dp题目，其背景虽然是子数组，和前面很多题目类似，但是涉及到等差数列，在考虑dp方程时使用到了hashmap。这道题目的分享到这里，感谢你的支持！
