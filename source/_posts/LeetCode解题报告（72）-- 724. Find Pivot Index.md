---
title: LeetCode解题报告（72）-- 724. Find Pivot Index
tags:
  - LeetCode
abbrlink: 18741
date: 2019-10-11 18:30:06
---

## Problem

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

<!-- more -->

**Example 1:**

```
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
```

**Example 2:**

```
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
```

**Note:**

- The length of `nums` will be in the range `[0, 10000]`.
- Each element `nums[i]` will be an integer in the range `[-1000, 1000]`.

------

## Analysis

&emsp;&emsp;这道题给定一个数组，要求我们找出一个位置，在这个位置左边的和与右边的和相等。网上看到有人使用头尾指针的做法（双向），但实际上这道题单向的思路也能做出来。首先要做一个转变：某个位置`i`的`左右两边和相等`**等价于**`总和 - nums[i] = 2 * 左边的和`。

&emsp;&emsp;所以我们首先求出总和，然后逐个遍历，计算是否满足上面的要求。如果满足则说明找到这个pivot index。

------

## Solution

&emsp;&emsp;易错点是判断的时候左边求和不需要加上当前元素。

------

## Code

```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int size = nums.size();
        if (size <= 2) {
            return -1;
        }
        int sum = 0;
        for (int i = 0; i < size; i++) {
            sum += nums[i];
        }
        int current = 0;
        for (int i = 0; i < size; i++) {
            if (sum - nums[i] == 2 * current) {
                return i;
            }
            current += nums[i];
        }
        return -1;
    }
};
```

------

## Summary

 &emsp;&emsp;数组类的题目很多时候需要转变一下判断条件，转换之后coding难度会大大下降。这道题的分享到这里，欢迎大家评论、转发，最后，谢谢您的支持！