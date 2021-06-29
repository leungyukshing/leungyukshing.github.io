---
title: LeetCode解题报告（267）-- 1658. Minimum Operations to Reduce X to Zero
tags:
  - LeetCode
mathjax: true
abbrlink: 5639
date: 2021-01-15 18:29:40
---

## Problem

You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this **modifies** the array for future operations.

Return *the **minimum number** of operations to reduce* `x` *to **exactly*** `0` *if it's possible**, otherwise, return* `-1`.

<!-- more -->

**Example 1:**

```
Input: nums = [1,1,4,2,3], x = 5
Output: 2
Explanation: The optimal solution is to remove the last two elements to reduce x to zero.
```

**Example 2:**

```
Input: nums = [5,6,7,8,9], x = 4
Output: -1
```

**Example 3:**

```
Input: nums = [3,2,20,1,1,3], x = 10
Output: 5
Explanation: The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`
- `1 <= x <= 109`

------

## Analysis

&emsp;&emsp;题目给出了一个数组`nums`和一个数字`X`，要求每次只能从数组的头或尾取出一个数字，然后用`X`减去这个数字，问最少需要多个步操作使得这个`X`被减成0。

&emsp;&emsp;这种题目很通用的解法就是前缀和+hashtable，求出前缀和记录到table中，然后逐个对比看看能不能相加为`X `，维护一个长度最小的即可。

&emsp;&emsp;但是这道题目有个更加简便的解法，这道题目要求我们要在这个数组中找出前面一部分和后面一部分求和，看看是否等于`X`。我们可以逆向思考一下，如果我们拿到数组的和`sum`，那么实际上就是在数组中找一个区间，这个区间的和等于`sum-X`。有了这个转换，我们就可以利用滑动窗口来找到这个区间。

------

## Solution

&emsp;&emsp;滑动窗口初始化`left`和`right`都是0，每次先移动right，然后比较区间里面的大小和`sum-X`，如果区间的值比较大，则需要右移`left`；如果刚好相等，则说明找到合适的区间，此时用总长度减去区间的长度就是操作的数量，维护这个最小值即可。

------

## Code

```c++
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        int numsSum = accumulate(nums.begin(), nums.end(), 0);
        int target = numsSum - x;
        
        int size = nums.size();
        int sum = 0; // sum of window
        int left = 0, right = 0;
        int result = INT_MAX;
        while (right < size) {
            sum += nums[right];
            while (left <= right && sum > target) {
                sum -= nums[left];
                left++;
            }
            if (sum == target) {
                result = min(result, size - (right - left + 1));
            }
            right++;
        }
        return result > size ? -1: result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题题目难度中等，但两种求解思路都非常值得总结。使用前缀和+hash table的方法是比较通用的，而使用滑动窗口机制则是比较巧妙的一个方法。这道题这道题目的分享到这里，谢谢您的支持！
