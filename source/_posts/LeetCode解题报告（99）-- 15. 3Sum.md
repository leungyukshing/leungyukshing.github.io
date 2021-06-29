---
title: LeetCode解题报告（99）-- 15. 3Sum
tags:
  - LeetCode
mathjax: true
abbrlink: 56579
date: 2020-07-10 21:20:11
---

## Problem

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

<!-- more -->

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

------

## Analysis

&emsp;&emsp;这道题目是非常经典的面试题，题目给定一个数组，然后找到和为0的三个数。之前在数组类型题目的题解博客中我也提到过，对于数组类题目很常用的一个做法就是先排序，我们可以利用大小顺序去做文章。这道题目也是一样的，先排序（默认从小到大）。然后我们来看如何满足三个数之和为0这个条件，我们把这个表示为$a + b + c = 0$。那么换一种表达方式就是$a + b = -c$ 。

&emsp;&emsp;看到后面这种表达方式，就知道前面为什么要排序了。当$c$确定的情况下，如果$a+b$比较大，就要选更小的数，反之就选择更大的数，所以在有序的数组上操作我们就知道要往哪个方向选择。采用双指针的方式，一前一后定位（front指针和back指针），如果$a+b$太小，则增加**front指针**，否则就减小$back$指针。

------

## Solution

&emsp;&emsp;整个过程要保证front指针小于back指针，同时在匹配上一个三元组之后，需要skip掉相同的数字，包括front和back。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > result;
        sort(nums.begin(), nums.end());
        
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int target = -nums[i];
            int front = i + 1;
            int back = size - 1;
            while (front < back) {
                if (nums[front] + nums[back] < target) {
                    front++;
                } else if (nums[front] + nums[back] > target) {
                    back--;
                } else {
                    vector<int> temp;
                    temp.push_back(nums[i]);
                    temp.push_back(nums[front]);
                    temp.push_back(nums[back]);
                    result.push_back(temp);
                    
                    front += 1;
                    back -= 1;
                    
                    while (front < back && nums[front] == nums[front - 1]) {
                        front++;
                    }
                    while (front < back && nums[back] == nums[back + 1]) {
                        back--;
                    }
                }
                
                
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度中等，是面试经常会考到的题目。它首先是一道数组类的题目，所以排序的做法是比较常见的，同时他还考察到了一些数学的知识，通过变换等式来简化程序逻辑。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
