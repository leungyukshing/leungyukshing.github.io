---
title: LeetCode解题报告（42）-- 41. First Missing Positive
tags:
  - LeetCode
abbrlink: 52619
date: 2018-12-03 14:28:23
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

**Note:**
Your algorithm should run in O(n) time and uses constant extra space.

## Analysis
&emsp;&emsp;这题的求解是建立在[645. Set Mismatch](http://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%8841%EF%BC%89--%20645.%20Set%20Mismatch.html)的基础上的，如果你没有做过这题，那么建议先做完这题之后，再来看本题。找丢失元素其实和找重复元素是一样的，都是先将数字放在对应的位置，不在自己位置上的就是有问题的数字。这题要找的是**没出现过的最小的正数**。同样按照`nums[i]`应该在`i+1`位置上的这个思路来处理数组，然后遍历数组，找到第一个不满足这个条件的下标，这个下标代表的值就是丢失的最小正数。

## Solution
&emsp;&emsp;Coding上没什么难度，按照分析的思路进行即可。注意当所有的元素都匹配了位置的时候，返回`size + 1`。

## Code
```C++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] <= 0 || nums[i] > nums.size() || nums[i] == i+1 || nums[i] == nums[nums[i] - 1])
                continue;
            swap(nums[i], nums[nums[i] - 1]);
            i--;
        }

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != i+1) {
                return i+1;
            }
        }
        return nums.size()+1;
    }
};
```
**运行时间：**约4ms，超过85.10%的CPP代码。

## Summary
&emsp;&emsp;这道题其实是综合了前面到题之后做的一个变形，本质上并没有很大的改变。我们的基本思路还是：将元素与位置一一对应，根据题目要求找出不符合要求的元素值或下标即可。本题的分析到此为止，谢谢！
