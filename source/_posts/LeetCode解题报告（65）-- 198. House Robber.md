---
title: LeetCode解题报告（65）-- 198. House Robber
tags:
  - LeetCode
abbrlink: 46177
date: 2019-09-30 18:13:33
---

## Problem

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

<!-- more -->

**Example 1:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

------

## Analysis

&emsp;&emsp;这道题是属于动态规划的，但是问法结合了一下情景，但实际上是非常简单的动态规划题目。题目说到，不能robber相邻的两间房子，言外之意就是对于第`i`个房子，如果你选择robber，则你不能robber第`i-1`个，那么你的总数就是要从第`i-2`个开始算；如果你不robber当前这个房子，那么总数就是和第`i-1`个是一样的。

------

## Solution

&emsp;&emsp;列好动态规划的状态转移公式直接coding即可。注意边界情况，如果房子数量是两个，我们直接选择价值较大的那个。

------

## Code

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if (size == 0) {
            return 0;
        }
        if (size == 1) {
            return nums[0];
        }
        if (size == 2) {
            return max(nums[0], nums[1]);
        }
        int f[size];
        f[0] = nums[0];
        f[1] = max(nums[0], nums[1]);
        for (int i = 2; i < size; i++) {
            f[i] = max(f[i - 1], f[i - 2] + nums[i]);
        }
        
        return f[size - 1];
    }
};
```

------

## Summary

&emsp;&emsp;本题是典型的动态规划题目，形式是：**当前的结果依赖于上上步的结果或上一步的结果**。对于这一类问题，我们只需要列出状态转移方程，然后处理边界情况即可。这道题的分享到这里结束了，欢迎大家转发、评论。最后，谢谢您的支持！
