---
title: LeetCode解题报告（170）-- 179. Largest Number
tags:
  - LeetCode
mathjax: true
abbrlink: 31478
date: 2020-09-25 17:29:08
---

## Problem

Given a list of non negative integers, arrange them such that they form the largest number.

<!-- more -->

**Example 1:**

```
Input: [10,2]
Output: "210"
```

**Example 2:**

```
Input: [3,30,34,5,9]
Output: "9534330"
```

**Note:** The result may be very large, so you need to return a string instead of an integer.

------

## Analysis

&emsp;&emsp;题目给出了一系列的数，要求我们利用这些数字组成一个最大的数。组成最大的数有一个基本的原则就是较大的数字放在高位。所以在比较两个数的时候就不能单纯比较他们之间的大小，而要**比较的是他们相互组成的数字的大小**。

------

## Solution

&emsp;&emsp;需要自定义排序，比较的方法是两个数字组成的数字（2种可能）。还要注意结果为多个0的情况，要特殊判断，直接返回一个0就可以了。

------

## Code

```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](int a, int b) {
            return to_string(a) + to_string(b) > to_string(b) + to_string(a);
        });
        int size = nums.size();
        string result = "";
        for (int i = 0; i < size; i++) {
            result += to_string(nums[i]);
        }
        return result[0] == '0'? "0": result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目无论是算法上还是coding层面都比较简单，构造大数问题还是要把握住高位优先的原则。这道题的分析到这里，谢谢！
