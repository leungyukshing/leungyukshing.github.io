---
title: LeetCode解题报告（456）-- 1150. Check If a Number Is Majority Element in a Sorted Array
mathjax: true
date: 2021-12-26 00:01:12
---

## Problem

Given an integer array `nums` sorted in non-decreasing order and an integer `target`, return `true` *if* `target` *is a **majority** element, or* `false` *otherwise*.

A **majority** element in an array `nums` is an element that appears more than `nums.length / 2` times in the array.

<!-- more -->

**Example 1:**

```
Input: nums = [2,4,5,5,5,5,5,6,6], target = 5
Output: true
Explanation: The value 5 appears 5 times and the length of the array is 9.
Thus, 5 is a majority element because 5 > 9/2 is true.
```

**Example 2:**

```
Input: nums = [10,100,101,101], target = 101
Output: false
Explanation: The value 101 appears 2 times and the length of the array is 4.
Thus, 101 is not a majority element because 2 > 4/2 is false.
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= nums[i], target <= 109`
- `nums` is sorted in non-decreasing order.

---

## Analysis

&emsp;&emsp;题目给出一个有序的数组`nums`和一个数`target`，要求判断`target`是不是一个majority，也就是说它出现的次数大于数组长度的一半。有关频率的题目最简单的做法就是遍历一遍，然后用map统计，最后看看频率是否大于数组长度的一半。但这里有一个更加快速的方法，因为这个数组是有序的，一般来说可以考虑二分查找。

&emsp;&emsp;那么这里二分查找怎么用呢？首先，`target`肯定是连续在一起的，所以我们可以找到它的左边界和右边界，这样就知道`target`一共出现了多少次，最后再做判断即可。二分查找找左、右边界是默写题了，这里不再赘述。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool isMajorityElement(vector<int>& nums, int target) {
        // left bound
        int left = 0, right = nums.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) {
                // go left
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        int left_bound = left;
        
        // right bound
        left = 0, right = nums.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                // go left
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        int right_bound = left - 1;
        //cout << left_bound << " " << right_bound << endl;
        return right_bound - left_bound + 1 > nums.size() / 2;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然是easy，但是其二分查找的解法实际上是综合了二分找左、右边界这两个知识点，是一道很有价值的题目。这道题目的分享到这里，感谢你的支持！
