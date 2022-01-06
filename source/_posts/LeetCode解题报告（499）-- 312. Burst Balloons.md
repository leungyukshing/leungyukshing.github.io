---
title: LeetCode解题报告（499）-- 312. Burst Balloons
mathjax: true
date: 2022-01-05 21:25:12
---

## Problem

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return *the maximum coins you can collect by bursting the balloons wisely*.

<!-- more -->

**Example 1:**

```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

**Example 2:**

```
Input: nums = [1,5]
Output: 10
```



**Constraints:**

- `n == nums.length`
- `1 <= n <= 500`
- `0 <= nums[i] <= 100`

---

## Analysis

&emsp;&emsp;这道题目给出一个数组`nums`，每次去除掉一个数字`nums[i]`，去除数字的cost是`nums[i - 1] * nums[i] * nums[i + 1]`。这道题目其实和之前多边形算三角形partition很类似，都是用dp，但是dp的定义有点不同。之前的题目我们可以限定`i`和`j`两个顶点，让`k`在`[i + 1, j - 1]`之间走，每次的cost就是`nums[i] * nums[k] * nums[j]`。

&emsp;&emsp;假设我们用同样的思路去设计，每次的cost是`nums[k - 1] * nums[k] * nums[k + 1]`吗？这看上去是正确，但是有问题。问题在于递归左右两边的时候`helper(i, k - 1)`和`helper(k + 1, j)`。我们只看左边`helper(i, k - 1)`，当左边部分把`nums[k - 1]`这个位置去除后，在计算去除`nums[k]`的时候就不能用`nums[k - 1] * nums[k] * nums[k + 1]`了，因为现在`nums[k - 1]`并不是`nums[k]`左边的数了，因为它已经被去除掉了。所以这里的定义要反过来，定义的是`k`这个数字最后被去除掉，所以我们递归处理左右两边，处理完之后，左边`[i, k - 1]`都被去除掉了，右边`[k + 1, j]`也被去除掉了，所以我就可以计算当前的cost为`nums[i - 1] * nums[k] * nums[j + 1]`。

## Solution

&emsp;&emsp;因为这道题目涉及到了两个边界的计算，为了使得代码逻辑统一处理，我们先在处理前在数组的前后各加一个1，所以处理的区间就是`[1, n - 2]`了。

------

## Code

```c++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(), 1);
        nums.push_back(1);

        int n = nums.size();
        memo = vector<vector<int>>(n, vector<int>(n, -1));
        return helper(nums, 1, n - 2);
    }
private:
    vector<vector<int>> memo;
    int helper(vector<int> &nums, int i, int j) {
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        int result = 0;
        for (int k = i; k <= j; ++k) {
            result = max(result, helper(nums, i, k - 1) + helper(nums, k + 1, j) + nums[i - 1] * nums[k] * nums[j + 1]);
        }
        return memo[i][j] = result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是非常经典的区间类型dp，其套路一般是确定一个范围，然后把范围中的每个值都遍历一遍，对每个位置的计算结果维护极大值或极小值，最后用来更新这个区间的结果（通常是一个记忆化数组，dp数组）。这道题目的分享到这里，感谢你的支持！

