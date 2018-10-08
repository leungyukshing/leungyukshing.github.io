---
title: LeetCode解题报告（十三）-- 496. Next Greater Element I
tags:
  - LeetCode
abbrlink: 53516
date: 2018-10-01 08:02:49
---
## Problem
You are given two arrays (**without duplicates**) `nums1` and `nums2` where `nums1`’s elements are subset of `nums2`. Find all the next greater numbers for `nums1`'s elements in the corresponding places of `nums2`.

The Next Greater Number of a number **x** in `nums1` is the first greater number to its right in `nums2`. If it does not exist, output -1 for this number.
<!-- more -->

**Example 1**:
```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```

**Example 2**:
```
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
```

**Note**:
  1. All elements in `nums1` and `nums2` are unique.
  2. The length of both `nums1` and `nums2` would not exceed 1000.

## Analysis
&emsp;&emsp;这道题涉及到两个问题，一个是找**next greater**，另一个是对应两个数组中的元素。第一个问题很简单，从当前元素的下一个开始遍历寻找即可。第二个问题的解决方法有很多，这里我使用了`map`来存放，因为它可以保证`key`的唯一性（这也是因为题目确保了在数组中元素是唯一的）。

## Solution
&emsp;&emsp;对于`nums2`中每一个元素，使用遍历的方法来寻找下一个比自己大的数字，然后使用`map`来存放这个值。接着遍历`num1`，每个元素打印自己在`map`中对应的`value`即可。

## Code
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            int nextGreaterNumber = -1;
            for (int j = i; j < nums.size(); j++) {
                if (nums[j] > nums[i]) {
                    nextGreaterNumber = nums[j];
                    break;
                }
            }
            m[nums[i]] = nextGreaterNumber;
        }

        vector<int> result;
        for (int i = 0; i < findNums.size(); i++) {
            result.emplace_back(m[findNums[i]]);
        }
        return result;
    }
private:
    map<int, int> m;
};
```
**运行时间**：约8ms，超过65.83%的CPP代码。

## Summary
&emsp;&emsp;这道题本身难度不大，但是使用明确的思路去解决对后面的题目至关重要，尤其是合理地使用`map`，在后续题目中会显的很重要。这题的分析到这里，谢谢您的支持！
