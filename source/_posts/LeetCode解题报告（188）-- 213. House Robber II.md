---
title: LeetCode解题报告（188）-- 213. House Robber II
tags:
  - LeetCode
mathjax: true
abbrlink: 25100
date: 2020-10-15 01:30:40
---

## Problem

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

<!-- more -->

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 3:**

```
Input: nums = [0]
Output: 0
```

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

------

## Analysis

&emsp;&emsp;这道题目是[House Robber](http://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%8865%EF%BC%89--%20198.%20House%20Robber.html)的后续题，题目的条件改成了房子是一个圈。这个条件的改动实际上只会影响头尾两个房子。之前的那道题目头尾是可以都打劫的，但是在这道题目的背景下，头和尾只能有一个被打劫。

&emsp;&emsp;既然除了头尾之外其余的逻辑都是一样的，那么我们可以借助之前那道题目的思路了。上面提到头和尾只能有一个被打劫，所以我们干脆就分两种情况，一种是只考虑头，一种是只考虑尾，那么两种结果都计算出来选比较大的那个就好了。

------

## Solution

&emsp;&emsp;dp的做法和之前那题一样，这里不赘述了。把dp的过程抽取出来单独成为一个方法，然后通过下标控制dp的范围，两种情况都计算好之后返回较大的值就可以了。

------

## Code

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() <= 1) {
            return nums.empty()? 0: nums[0];
        }
        return max(helper(nums, 0, nums.size() - 1), helper(nums, 1, nums.size()));
    }
private:
    int helper(vector<int>& nums, int begin, int end) {
        if (end - begin <= 1) {
            return nums[begin];
        }
        vector<int> dp(end, 0);
        dp[begin] = nums[begin];
        dp[begin + 1] = max(dp[begin], nums[begin + 1]);
        
        for (int i = begin + 2; i < end; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[end - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目dp的一个小变形，如果说做过第一题的话，那么做这题将是非常简单的。但是如果没有前面的准备，直接做这一题的话，思考的难度会增大很多。所以遇到这类题目，我们不妨先考虑最基本的情况，比如吗，解决环状的情况可以先考虑解决线性的情况，然后再对特殊情况处理。这道题目的分享到这里，谢谢！
