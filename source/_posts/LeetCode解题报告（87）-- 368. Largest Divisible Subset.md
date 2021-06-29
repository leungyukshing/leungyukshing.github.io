---
title: LeetCode解题报告（87）-- 368. Largest Divisible Subset
tags:
  - LeetCode
mathjax: true
abbrlink: 17325
date: 2020-06-14 13:18:22
---

## Problem

Given a set of **distinct** positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies:

Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

<!-- more -->

**Example 1:**

```
Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)
```

**Example 2:**

```
Input: [1,2,4,8]
Output: [1,2,4,8]
```

------

## Analysis

&emsp;&emsp;这道题目要求的时候计算最大可整除的集合，在这个集合里面任意两个元素都可以满足整除关系。首先我们先看整除关系有什么特点，然后再看如何去遍历数组得到答案。

&emsp;&emsp;整除关系应用到代码里面就是取余为0。取余操作有个特点就是，小的数对大的数取余的结果就是小的数本身，又因为题目说了给定的数组的数字都是不重复的，所以用小的数对大的数取余这种是一定得不到最大的集合的。那么就只剩下大的数对小的数取余了。所以我们可以先对数组进行**排序**，然后**从大的方向**开始遍历。

&emsp;&emsp;我们确定了从大的位置开始遍历，然后对于每一个数，再遍历比他大的数字。因为我们需要维护的是最大的集合，而且数组的元素有的选，有的不选，这个看上去和**最长匹配子串**有点相似，所以可以考虑使用DP。`dp[i]`表示到`nums[i]`位置最大可整除的子集合的**长度**。这个长度信息是用来控制遍历的，而不是用来计算结果的。我们需要另外一个数组来存放计算结果，因为结果可能有重复，所以我们不能直接把结果放入到数组，而是存放能整除这个数的数字下标，最后在通过下标回溯。`max_val` 记录的是总的最大的长度，`max_idx`记录的是最开头起始数字的下标。

------

## Solution

&emsp;&emsp;数组使用vector实现，其余的根据上面的分析进行就可以了。

------

## Code

```c++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int size = nums.size();
        vector<int> dp(size, 0), parent(size, 0), res;
        int max_val = 0, max_idx = 0;
        
        for (int i = size - 1; i >= 0; i--) {
            for (int j = i; j < size; j++) {
                if (nums[j] % nums[i] == 0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    parent[i] = j;
                    if (max_val < dp[i]) {
                        max_val = dp[i];
                        max_idx = i;
                    }
                }
            }
        }
        
        for (int i = 0; i < max_val; i++) {
            res.push_back(nums[max_idx]);
            max_idx = parent[max_idx];
        }
        return res;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目是融合了取余计算的动态规划问题。看上去第一反应会感到比较复杂，但是分析后先确定遍历的方向，然后就是要维护一个结果的数组，这里比较难的是这个数组的维护并不是直接维护一个数组，而是维护一个**类似于指针的数组**，每个位置都指向的是能整除自己的数的下标，用于串联起整个集合。这道题目的分享到此为止，感谢您的支持，欢迎转发、评论，分享，谢谢！
