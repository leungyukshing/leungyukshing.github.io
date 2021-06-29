---
title: LeetCode解题报告（121）-- 949. Largest Time for Given Digits
tags:
  - LeetCode
mathjax: true
abbrlink: 44396
date: 2020-09-01 18:33:51
---

## Problem

Given an array of 4 digits, return the largest 24 hour time that can be made.

The smallest 24 hour time is 00:00, and the largest is 23:59.  Starting from 00:00, a time is larger if more time has elapsed since midnight.

Return the answer as a string of length 5.  If no valid time can be made, return an empty string.

<!-- more -->

**Example 1:**

```
Input: [1,2,3,4]
Output: "23:41"
```

**Example 2:**

```
Input: [5,5,5,5]
Output: ""
```

**Note:**

1. `A.length == 4`
2. `0 <= A[i] <= 9`

------

## Analysis

&emsp;&emsp;题目给出了四个数字，要求利用这四个数字组合出最大的时间。最直接的思路就是把各种组合都遍历一边，然后在符合时间格式的范围内选一个最大的。

&emsp;&emsp;先看一下时间的两个限制，小时小于等于23，分小于等于59。然后就要看如何遍历了，因为我们希望从小到大遍历，这样只需要维护一个最大的时间就可以了。对于四个数字的组合遍历，这里可以用到STL提供的`next_permutation`。

&emsp;&emsp;当然遍历也可以不使用STL提供的，可以自己用三重循环实现一个。原理也很简单，3重循环去产生四个下标，下标之间互不相同，分别选取数组中的一个数字，然后验证组成时间的合法性。在这种做法下因为遍历是无序的，所以还需要判断生成的时间比result要大才更新。

------

## Solution

&emsp;&emsp;因为我们希望从小到大遍历所有的排列，所以要先排序。

------

## Code

```c++
class Solution {
public:
    string largestTimeFromDigits(vector<int>& A) {
        sort(A.begin(), A.end());
        string result;
        
        do {
            string hour = {char(A[0] + '0'), char(A[1] + '0')};
            string minute = {char(A[2] + '0'), char(A[3] + '0')};
            if (hour <= "23" && minute <= "59") {
                result = hour + ":" + minute;
            }
        } while (next_permutation(A.begin(), A.end()));
        
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题难度不大，值得总结的点是用到了C++ STL的`next_permuatation`，在刷题的时候能够借用工具也是一种能力的体现。这道题的分析到这里，谢谢！
