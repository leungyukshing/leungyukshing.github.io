---
title: LeetCode解题报告（321)-- 376. Wiggle Subsequence
tags:
  - LeetCode
mathjax: true
abbrlink: 22706
date: 2021-03-22 23:06:30
---

## Problem

Given an integer array `nums`, return *the length of the longest **wiggle sequence***.

A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with fewer than two elements is trivially a wiggle sequence.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` are alternately positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences, the first because its first two differences are positive and the second because its last difference is zero.

A **subsequence** is obtained by deleting some elements (eventually, also zero) from the original sequence, leaving the remaining elements in their original order.

<!-- more -->

**Example 1:**

```
Input: nums = [1,7,4,9,2,5]
Output: 6
Explanation: The entire sequence is a wiggle sequence.
```

**Example 2:**

```
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].
```

**Example 3:**

```
Input: nums = [1,2,3,4,5,6,7,8,9]
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`

 

**Follow up:** Could you solve this in `O(n)` time?

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求我们找出最长的摆动子序列。摆动的定义是两者之差在正负摆动。首先这类在数组中找子序列长度的题目一般就采用dp来做，但是这里的摆动怎么定义状态呢？实际上这个和股票买卖是有点类似的，一天买一天卖。我们分别定义两个方程：

- `up[i]`表示第`i`个数字处于上摆时，所能构成的摆动序列的最大长度；
- `down[i]`表示第`i`个数字出于下摆时，所能构成的摆动序列的最大长度。

&emsp;&emsp;这样一来，我们就可以根据前一个位置来推出后一个位置的值了。我们根据前后两个数字值的大小关系来分别处理：

1. `nums[i] > nums[i - 1]`，说明后一个位置大，所以是上摆，结果就是前一个位置处于下摆的值加上1。同时因为这个位置不能下摆，所以下摆的值和前一个位置保持一致；
2. 同理`nums[i] < nums[i - 1]`，说明前一个位置大，所以是下摆，结果就是前一个位置处于上摆的值加上1。因为这个位置不能上摆，所以上摆的值和前一个位置上摆的值保持一直。
3. 如果两者相等，则和前一个位置的值都保持一致。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int size = nums.size();
        vector<int> up(size, 0), down(size, 0);
        up[0] = down[0] = 1;
        for (int i = 1; i < size; i++) {
            if (nums[i] > nums[i - 1]) {
                up[i] = down[i - 1] + 1;
                down[i] = down[i - 1];
            } else if (nums[i] < nums[i - 1]) {
                down[i] = up[i - 1] + 1;
                up[i] = up[i - 1];
            } else {
                down[i] = down[i - 1];
                up[i] = up[i - 1];
            }
        }
        return max(down[size - 1], up[size - 1]);
    }
};
```

------

## Summary

&emsp;&emsp;这道题也算是比较简单的dp题目，这类上下摆动、买卖的题目，很明显的特征就是，不但依赖于前一个状态，还要判断不同情况的处理逻辑。解决这类问题使用两个动态转移方程即可。这道题目的分享到这里，感谢你的支持！
