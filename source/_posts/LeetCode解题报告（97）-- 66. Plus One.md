---
title: LeetCode解题报告（97）-- 66. Plus One
tags:
  - LeetCode
date: 2020-07-06 22:12:31
mathjax: true

---

## Problem

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

<!-- more -->

**Example 1:**

```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**

```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

------

## Analysis

&emsp;&emsp;这道题目是很经典的数字加法题目，其原理就是加法进位。这道题目是比较简单，只需要加1，所以我们只需要考虑是否进位就可以了。我们先计算某一位上的`sum`，**通过除法判断是否需要进位，通过取余来获取当前位的值**。最后如果还有进位，就需要在最前面加1。

------

## Solution

&emsp;&emsp;用一个`sum`变量表示当前位的数值，用`add`表示进位。

------

## Code

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int sum = 0;
        int add = 1;
        int size = digits.size();
        
        for (int i = size - 1; i >= 0; i--) {
            sum = add + digits[i];
            add = sum / 10;
            sum %= 10;
            digits[i] = sum;
        }
        
        if (add > 0) {
            digits.insert(digits.begin(), 1);
        }
        return digits;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本身难度不算大，重要的是背后的加法逻辑。之后的大数加法或者链表的加法用到的思路都是相似的。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
