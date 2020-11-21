---
title: LeetCode解题报告（221）-- 33. Search in Rotated Sorted Array
tags:
  - LeetCode
mathjax: true
date: 2020-11-21 10:18:05

---

## Problem

You are given an integer array `nums` sorted in ascending order, and an integer `target`.

Suppose that `nums` is rotated at some pivot unknown to you beforehand (i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

*If target is found in the array return its index, otherwise, return -1.*

<!-- more -->

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

**Constraints:**

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- All values of `nums` are **unique**.
- `nums` is guranteed to be rotated at some pivot.
- `-10^4 <= target <= 10^4`

------

## Analysis

&emsp;&emsp;这道题是一道数组搜索相关的题目，但是背景的设置增加了一些难度。题目给出的数组是部分有序的（这个数组原来是一个从小到大有序的数组，然后在某个位置进行了旋转），这个难度就提升了。

&emsp;&emsp;在分析问题的时候，我们还是先从基础入手。假如给定的是一个整体有序的数组，那我们会怎么处理呢？毫无疑问首选的就是二分搜索了。那我们就来看看二分搜索放到这个题目中会有什么问题。以Example1为例，`[4,5,6,7,0,1,2]`中搜索`0`，二分时我们先拿到中间的元素`7`，这个时候和target值对比，发现target小于7。这个时候如果是整体有序的时候，我们就会直接到左半边搜索了，但是这道题目就会遇到问题了。我们可以先判断左右两边的有序性。7比4大，所以7的左半边（包含7）是有序的；但是7比2也大，所以右半边（包含7）就是无序的，所以我们可以利用左半边的有序性进行二分。如果target落在了左半边，那么直接到左半边找就可以，反之就肯定在右半边，而二分到右半部分的时候，实际上也是有序了，后面的二分步骤就和正常情况的没有区别。

&emsp;&emsp;  所以这里的特殊处理就是要判断左右两部分的有序性，利用有序的部分进行判断，如果不在有序的部分，就直接在另一部分找。

------

## Solution

&emsp;&emsp;判断区间的时候注意边界值，取等于的情况要特别注意。

------

## Code

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            // cout << mid << endl;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] < nums[right]) {
                // right part is sorted
                // cout << "right" << endl;
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            } else {
                // left part is sorted
                // cout << "left" << endl;
                if (nums[mid] > target && target >= nums[left]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上是二分搜索的一个升级版，需要对二分搜索有比较清晰的认识。二分搜索的思想实际上就是不断缩窄搜索的范围，而这道题目需要特殊处理的地方就在这个判断范围，其余的和正常的二分搜索并无区别。这道题目的分享到这里，谢谢！
