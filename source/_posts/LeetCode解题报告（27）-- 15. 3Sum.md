---
title: LeetCode解题报告（27）-- 15. 3Sum
tags:
  - LeetCode
abbrlink: 61130
date: 2018-10-30 22:13:13
---
## Problem
Given an array `nums` of n integers, are there elements a, b, c in `nums` such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

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

## Analysis
&emsp;&emsp;这道题是比较难的一道题，题目要求很简单，在给出的数组中找出三个数，它们的和为0。最简单直接的做法就是三重循环暴力破解，求解出所有的组合，可是这种算法的复杂度是极高的，我们是不能够接受这种复杂度的。
&emsp;&emsp;我们可以先来分析一下什么情况下三个数的和能够为0？首先要求的是三个数中要有正数和负数。其次是两个数的和是第三个数的相反数。根据这两个很简单的规则，我们就可以优化算法了。
&emsp;&emsp;先对数组进行排序，这是为了我们可以利用第一个规则。排序后，当我们检测到当前元素为正数，其后面的元素也一定为正数，自然也不可能和为0了，可以直接结束判断。
&emsp;&emsp;然后我们要怎么利用第二个规则呢？比如说我们遍历到当前元素`nums[i]`，如果它能和数组中另外两个元素的和为0，那么它是两个数的和的相反数。我们需要在`nums[i]`后面的数中找到这两个数。这里的寻找采用一个类似于二分搜索的策略以提高搜索速度。同时使用`begin`和`end`两个指针指针，分别指向`nums[i]`后的第一个元素和最后一个元素，然后比较三个数的和。若`sum == 0`，则我们找到这三个数，存入结果；若`sum > 0`，说明最后的数选大了，`end--`；若`sum < 0`，说明前面的数选小了，`begin++`。

## Solution
&emsp;&emsp;使用系统自带的`sort`函数对给定的数组排序，然后按照上面给出的思路遍历数组。注意，成功找到一组之后，需要排除掉重复的项，以进行下一次的检测。

## Code
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int size = nums.size();
        sort(nums.begin(), nums.end());

        int begin, end;
        for (int i = 0; i < size - 2; i++) {
            if (nums[i] > 0)
                break;
            if (i > 0 && nums[i - 1] == nums[i])
                continue;

            begin = i + 1;
            end = size - 1;

            while (begin < end) {
                int sum = nums[i] + nums[begin] + nums[end];
                if (sum == 0) {
                    vector<int> temp;
                    temp.emplace_back(nums[i]);
                    temp.emplace_back(nums[begin]);
                    temp.emplace_back(nums[end]);
                    result.emplace_back(temp);
                    begin++;
                    end--;
                    while (begin < end && nums[begin-1] == nums[begin]) {
                        begin++;
                    }
                    while (begin < end && nums[end] == nums[end + 1]) {
                        end--;
                    }
                }
                else if (sum > 0) {
                    end--;
                }
                else {
                    begin++;
                }
            }
        }
        return result;
    }
};
```
**运行时间**：约84ms，超过71.09%的CPP代码。

## Summary
&emsp;&emsp;这道题的意思很简单，但是要找出快速的求解方法还是有一定的难度。我的体会是，在处理这种问题的时候要先分析出问题的特点，像这道题，只要找到三个数相加的一些条件，就可以对算法进行优化。这道题的分析就到这里，谢谢您的支持！
