---
title: LeetCode解题报告（246）-- 334. Increasing Triplet Subsequence
tags:
  - LeetCode
mathjax: true
date: 2020-12-19 01:45:48


---

## Problem

Given an integer array `nums`, return `true` *if there exists a triple of indices* `(i, j, k)` *such that* `i < j < k` *and* `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4,5]
Output: true
Explanation: Any triplet where i < j < k is valid.
```

**Example 2:**

```
Input: nums = [5,4,3,2,1]
Output: false
Explanation: No triplet exists.
```

**Example 3:**

```
Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`

 

**Follow up:** Could you implement a solution that runs in $O(n)$ time complexity and $O(1)$ space complexity?

------

## Analysis

&emsp;&emsp;这道题目要求我们判断一个数组中是否存在长度为3递增的子序列。题目的Follow up要求的是线性计算复杂度不使用额外的空间，所以就必须要在一次遍历中完成。观察题目发现，题目只要求我们返回能否，而不需要记录下所有的可能答案，因为长度刚好是三就可以了，所以我们直接就用变量去记。从前往后遍历，用两个变量记录下最小值和第二小的值，如果某个数比这两个值都要大，那么就说明找到了递增的子序列，直接return true。

------

## Solution

&emsp;&emsp;用`m1`表示最小值，用`m2`表示第二小的值。首先尝试更新`m1`，如果比`m1`大，再尝试更新`m2`，如果还比`m2`大，就说明至少有长度为3的递增子序列，直接return true即可。

------

## Code

```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int m1 = INT_MAX, m2 = INT_MAX;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (m1 >= nums[i]) {
                m1 = nums[i];
            } else if (m2 >= nums[i]) {
                m2 = nums[i];
            } else {
                return true;
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本身并不复杂，难点在于增加了时间和空间复杂度限制后，在做题方式上要有相对应的改进。一般来说线性的遍历可以通过双指针来实现，因为这道题目关注的是数字的值，所以就用两个变量记录，如果是关注下标，可以用指针。这道题目的分享到这里，谢谢您的支持！
