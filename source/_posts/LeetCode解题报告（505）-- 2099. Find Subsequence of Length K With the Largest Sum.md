---
title: LeetCode解题报告（505）-- 2099. Find Subsequence of Length K With the Largest Sum
mathjax: true
date: 2022-01-10 22:09:56
---

## Problem

You are given an integer array `nums` and an integer `k`. You want to find a **subsequence** of `nums` of length `k` that has the **largest** sum.

Return ***any** such subsequence as an integer array of length* `k`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

<!-- more -->

**Example 1:**

```
Input: nums = [2,1,3,3], k = 2
Output: [3,3]
Explanation:
The subsequence has the largest sum of 3 + 3 = 6.
```

**Example 2:**

```
Input: nums = [-1,-2,3,4], k = 3
Output: [-1,3,4]
Explanation: 
The subsequence has the largest sum of -1 + 3 + 4 = 6.
```

**Example 3:**

```
Input: nums = [3,4,3,3], k = 2
Output: [3,4]
Explanation:
The subsequence has the largest sum of 3 + 4 = 7. 
Another possible subsequence is [4, 3].
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `-105 <= nums[i] <= 105`
- `1 <= k <= nums.length`

---

## Analysis

&emsp;&emsp;这道题目给出一个数组，要求求出拥有最大和的长度为`k`的子序列。因为是子序列，不是子数组，所以不考虑sliding window。实际上就是取前`k`大的数，但是题目要求返回的是数组，而且元素之间的相对位置要保持不变。所以我们就不能只用一个数组来做了。首先copy一个数组出来，然后排序，取前`k`大的数放到`unordered_map`中，然后再遍历一遍原数组，只有在`unordered_map`中出现过的才放到结果中，注意动态更新，这样就能按照原来的相对顺序得到答案。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> maxSubsequence(vector<int>& nums, int k) {
        vector<int> temp = nums;
        sort(nums.rbegin(), nums.rend());
        unordered_map<int, int> m;
        for (int i = 0; i < k; ++i) {
            ++m[nums[i]];
        }
        
        vector<int> result;
        for (int num: temp) {
            if (m[num]-- > 0) {
                result.push_back(num);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然不难，但是如果没有做过的话，一下子还是有点没想过来。这道题目的分享到这里，感谢你的支持！

