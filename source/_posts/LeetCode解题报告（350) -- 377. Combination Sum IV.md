---
title: LeetCode解题报告（350) -- 377. Combination Sum IV
tags:
  - LeetCode
mathjax: true
abbrlink: 12595
date: 2021-04-23 01:30:40
---

## Problem

Given an array of **distinct** integers `nums` and a target integer `target`, return *the number of possible combinations that add up to* `target`.

The answer is **guaranteed** to fit in a **32-bit** integer.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```

**Example 2:**

```
Input: nums = [9], target = 3
Output: 0
```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 1000`
- All the elements of `nums` are **unique**.
- `1 <= target <= 1000`

 

**Follow up:** What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

------

## Analysis

&emsp;&emsp;题目给出一个数组，还有一个`target`值，通过在数组中选取数字，使得这些数字的和等于`target`，问这样的组合有多少个？其中数组中的数字是可以重复使用的。这种类型的题目暴力解法大概率都会超时的，所以就要想一个优化的方法。

&emsp;&emsp;题目特意说到不同的排列也算不同，就是说`[1,1,2]`和`[2, 1, 1]`达到的效果是相同的，但是算两种。这类型题目主流的优化思路都是记录中间就算状态，也就是dp了。因为求目标值，所以直接定义`dp[i]`为和为`i`的排列数量。这样定义的好处是，无论怎么排列，都会在前面都计算好，比如说给出的数组是`[1, 2, 3]`，和为3的排列`dp[3]`就包括了`[1,1,1]`、`[1,2]`、`[2,1]`和`[3]` 这些情况，当我们计算`dp[4]`的时候，就可以直接利用`dp[3]`来计算了。

------

## Solution

&emsp;&emsp;根据上面的思路，我们需要构建的是`[1,target]`的dp值。每一个都需要遍历给出的数组，然后通过`i - num`找到在前面计算过的dp值加上。

------

## Code

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned int> dp(target+1, 0);
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (auto &num: nums) {
                if (i - num >= 0) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道比较典型的dp题目，这道题目其实dp的痕迹很明显，但是计算会需要一点技巧。实际上我们只需要从小到大计算dp值即可，每次都遍历数组中的元素。这道题目的分享到这里，感谢你的支持！
