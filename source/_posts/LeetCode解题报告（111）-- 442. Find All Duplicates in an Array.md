---
title: LeetCode解题报告（111）-- 442. Find All Duplicates in an Array
tags:
  - LeetCode
mathjax: true
abbrlink: 63405
date: 2020-08-08 12:39:28
---

## Problem

Given an array of integers, 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear **twice** and others appear **once**.

Find all the elements that appear **twice** in this array.

Could you do it without extra space and in O(*n*) runtime?

<!-- more -->

**Example:**

```
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

------

## Analysis

&emsp;&emsp;题目要求在数组中找到重复出现的数字，之前Single Number的题目是**找只出现一次的数字**，我们可以借助异或运算把相同的元素抵消掉。但是这道题目要找重复出现的，显然就要换一种思路了。无脑暴力遍历的方法这里就不介绍了，题目更进一步的要求是在$O(n)$的时间复杂度内解决，我们就往着这个目标走。

&emsp;&emsp;需要注意到题目给出了一个限制条件$a \le a[i] \le n$，在数组类型的题目中，一旦我们有这种前提条件，可以大胆地考虑类似数组指针的做法。我们把一个元素的值用作数组的下标，这样的逻辑下，如果是相同的元素，就会映射到同一个下标位置。同时，题目的条件是重复出现的数字会重复出现两次，判断两次一个很好的条件是使用**正负关系**。当第一次通过下标访问的时候，把该位置上门的元素设置为相反数。如果下一次还访问到一个负数，则说明这个下标是重复出现的，我们把这个值加到答案中就可以了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> result;
        int size = nums.size();
        
        for (int i = 0; i < size; i++) {
            int index = abs(nums[i]);
            if (nums[index - 1] < 0) {
                result.push_back(index);
            } else {
                nums[index - 1] = -nums[index - 1];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目给了我们两个启示。一是在某种特定条件下，可以利用数组指针的思路去解题；二是处理二次重复问题，可以使用正负关系。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
