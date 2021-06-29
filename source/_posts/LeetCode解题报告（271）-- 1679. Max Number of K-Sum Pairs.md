---
title: LeetCode解题报告（271）-- 1679. Max Number of K-Sum Pairs
tags:
  - LeetCode
mathjax: true
abbrlink: 62412
date: 2021-01-19 01:43:14
---

## Problem

You are given an integer array `nums` and an integer `k`.

In one operation, you can pick two numbers from the array whose sum equals `k` and remove them from the array.

Return *the maximum number of operations you can perform on the array*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4], k = 5
Output: 2
Explanation: Starting with nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
There are no more pairs that sum up to 5, hence a total of 2 operations.
```

**Example 2:**

```
Input: nums = [3,1,3,4,3], k = 6
Output: 1
Explanation: Starting with nums = [3,1,3,4,3]:
- Remove the first two 3's, then nums = [1,4,3]
There are no more pairs that sum up to 6, hence a total of 1 operation.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `1 <= k <= 109`

------

## Analysis

&emsp;&emsp;这道题目一看上去有点Two Sum的感觉，实际上就是问我们有多少个Two Sum的组合。还是采用类似的做法，用map来存储每个数字出现的次数。然后从头开始找匹配的元素，仅当`nums[i]`和`k - nums[i]`都有的时候，才能构成一对组合。

------

## Solution

&emsp;&emsp;特别需要注意`nums[i] + nums[i] = k`的情况，因为此时`nums[i]`和`k - nums[i]`都出现了一次，实际上他们是同一个，所以要考虑这种edge case。

------

## Code

```c++
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        map<int, int> m;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            m[nums[i]]++;
        }
        
        int result = 0;
        for (int i = 0; i < size; i++) {
            if (m[nums[i]] < 1 || m[k - nums[i]] < 1 + (nums[i] * 2 == k)) {
                continue;
            }
            m[nums[i]]--;
            m[k - nums[i]]--;
            result++;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;如果做过Two Sum的朋友（该不会还有人没做过吧），解决这道题目应该是易如反掌，使用HashTable解决数组中数对问题是常用手段，这里也当作是一次强化练习了。这道题这道题目的分享到这里，谢谢您的支持！
