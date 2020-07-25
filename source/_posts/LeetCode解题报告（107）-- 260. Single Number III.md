---
title: LeetCode解题报告（107）-- 260. Single Number III
tags:
  - LeetCode
date: 2020-07-23 23:57:21
mathjax: true

---

## Problem

Given an array of numbers `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

<!-- more -->

**Example:**

```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```

**Note**:

1. The order of the result is not important. So in the above example, `[5, 3]` is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

------

## Analysis

&emsp;&emsp;这道题目Single Number的又一个变种。之前的两道题的重要思路都是用**异或**，这道题也不例外。但是这道题的难点在于如何找到两个出现一次的数字。如果是把数组里面的所有元素都异或，那么最后的结果一定就是这两个数字异或的结果，那么我们要做的就是把这两个区分出来。我们来研究这个结果，它是由两个只出现一次的数字异或得到的，所以这个数字为1的位，就是两个数字不同的位。所以我们就可以利用这个特点，区分出这两个数字。抽取出这一位，然后和数组中其余的数字再做异或，对于出现两次的数字，该位自然能够异或成0，而对于只出现一次的数字，有一个会异或成1，有一个会异或成0，这样就能够区分两个数字了。

------

## Solution

&emsp;&emsp;这里用到了`accumualte`方法，它常用于数组中累加或者自定义函数的实现，与python比较类似。然后我们可以通过`diff &= -diff`来计算出最右端为1的数，用于后面计算出两个只出现一次的数。

------

## Code

```c++
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int diff = accumulate(nums.begin(), nums.end(), 0, bit_xor<int>());
        diff &= -diff;
        vector<int> res(2, 0);
        for (auto &a : nums) {
            if (a & diff) res[0] ^= a;
            else res[1] ^= a;
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是single number的变种，难度比起前两个都比较大。如果前面两题都做了的话，对于异或比较熟悉会好下手，关键是利用异或的特点，从数字的每一位出发去寻找不同的数字。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
