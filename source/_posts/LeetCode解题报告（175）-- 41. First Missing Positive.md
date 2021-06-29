---
title: LeetCode解题报告（175）-- 41. First Missing Positive
tags:
  - LeetCode
mathjax: true
abbrlink: 60992
date: 2020-09-30 15:34:35
---

## Problem

Given an unsorted integer array, find the smallest missing positive integer.

<!-- more -->

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Follow up:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

------

## Analysis

&emsp;&emsp;这道题目的意思非常简单，如果没有时间和空间的限制，相信大家都能很快做出来。第一种方法是排序，排序后从头开始遍历，找到不连续的即可。第二种做法是用额外的空间，多使用一个map来存放，然后从1开始遍历这个map，找到缺失的那个元素。然而题目对时间和空间的复杂度都加了限制，要求时间复杂度是`O(n)`，空间复杂度是`O(1)` ，所以这两种方法都不符合要求了。

&emsp;&emsp;还记得之前数组类型的题目我介绍过，找缺失的元素，同时不使用额外空间，一个很重要的方法就是用数字本身的值作为下标。在这道题目中，数字`1`应该出现在下标为0的位置，数字`2`应该出现在下标为1的位置，以此类推。按照这种方法，把数组放到它该放的位置，这样再从头遍历一遍，就知道缺了哪个位置了。

------

## Solution

&emsp;&emsp;在数据结构上这道题没有用到额外的空间。

------

## Code

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int size = nums.size();
        if (size == 0) {
            return 1;
        }
        for (int i = 0; i < size; i++) {
            if (nums[i] > 0 && nums[i] < size && nums[nums[i] - 1] != nums[i]) {
                // swap
                int temp = nums[i];
                nums[i] = nums[temp - 1];
                nums[temp - 1] = temp;
                i--;
            }
        }
        
        for (int i = 0; i < size; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return size + 1;
    }
};
```

------

## Summary

&emsp;&emsp;这种题目要求解不难，但是要在空间和时间都有限制的情况下解决，就需要用一些技巧了。这道题的分析到这里，谢谢！
