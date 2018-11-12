---
title: LeetCode解题报告（29）-- 581. Shortest Unsorted Continuous Subarray
tags:
  - LeetCode
abbrlink: 47984
date: 2018-11-06 10:18:02
---
## Problem
Given an integer array, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the **shortest** such subarray and output its length.
<!-- more -->

**Example 1**:
```
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

**Note**:
  + Then length of the input array is in range `[1, 10,000]`.
  + The input array may contain duplicates, so ascending order here means <=.

## Analysis
&emsp;&emsp;这道题目的要求是：要我们在给定的一个字符串中，找到长度最小的子串，对这个子串按升序排序后，整个字符串是升序的。看完题目的第一反应是这个问题很复杂，因为既要找出这种字符串，又要是最短的。但是仔细看了一下题目给出的例子，发现有一个很重要的特点：如果只将该子串排序就可以得到升序的串，那么该串上位置改变的串就是我们要找的最短子串！

## Solution
&emsp;&emsp;按照上面的思路，我们先建立一份拷贝，对其进行排序，然后使用`start`和`end`两个下标分别从头和尾向中间搜索，找到与原字符串不等就停止，最后找出来的子串长度就是`end - start + 1`。特别注意，如果本身的字符串已经是有序，那么`start == end`退出循环，这里要另外加一个判断来处理原字符串本身有序的情况。

## Code
```C++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        if (nums.size() <= 1) {
            return 0;
        }
        vector<int> temp = nums;
        sort(temp.begin(), temp.end());
        int start = 0, end = nums.size() - 1;
        while (start < end) {
            if (nums[start] == temp[start]) {
                start++;
            }
            if (nums[end] == temp[end]) {
                end--;
            }
            if (nums[start] != temp[start] && nums[end] != temp[end]) {
                break;
            }
        }
        if (start == end) {
            return 0;
        }
        return (end - start + 1);
    }
};
```
**运行时间：**约36ms，超过约51.05%的CPP代码。

## Summary
&emsp;&emsp;这道题本身的难度不大，难在如何将题目要求简化，理解到这个问题的本质。如果采用逐个字符检测的话，工作量就会很繁重而且很容易出错。我们要善于利用系统的排序函数来帮助我们解决问题。这道题的分析到这里，谢谢！
