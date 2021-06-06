---
title: LeetCode解题报告（387) -- 164. Maximum Gap
tags:
  - LeetCode
mathjax: true
date: 2021-06-06 16:57:33

---

## Problem

Given an integer array `nums`, return *the maximum difference between two successive elements in its sorted form*. If the array contains less than two elements, return `0`.

You must write an algorithm that runs in linear time and uses linear extra space.

<!-- more -->

**Example 1:**

```
Input: nums = [3,6,9,1]
Output: 3
Explanation: The sorted form of the array is [1,3,6,9], either (3,6) or (6,9) has the maximum difference 3.
```

**Example 2:**

```
Input: nums = [10]
Output: 0
Explanation: The array contains less than 2 elements, therefore return 0.
```



**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目提示了就需要排序，但是又限定了线性的空间复杂度和时间复杂度，只能用桶排序了。要先确定桶的容量和桶的数量：

- 桶的容量 = （最大值 - 最小值）/ 个数 + 1；
- 桶的个数 = （最大值 - 最小值）/ 桶的容量 + 1。

&emsp;&emsp;然后需要在每个桶都找到一个极大值和极小值，因为最大间距的两个数不会存在同一个桶中（这是由上面的计算方式决定的，这里没有严谨的推导），所以结果必然是后一个个桶的最小值和前一个桶的最大值的差。

------

## Solution

&emsp;&emsp;因为我们不是需要整个排序的结果，所以没有必要实现完整的桶排序，所以我们只需要记录每个桶的极大值和极小值，最后把这些极大值、极小值都处理一遍即可。

------

## Code

```c++
class Solution {
public:
    int maximumGap(vector<int>& nums) {
        if (nums.size() <= 1) {
            return 0;
        }
        int mx = INT_MIN, mn = INT_MAX, size = nums.size();
        
        for (int num: nums) {
            mx = max(mx, num);
            mn = min(mn, num);
        }
        
        int capacity = (mx - mn) / size + 1, cnt = (mx - mn) / capacity + 1;
        
        vector<int> bucket_min(cnt, INT_MAX), bucket_max(cnt, INT_MIN);
        for (int num: nums) {
            int idx = (num - mn) / capacity;
            bucket_min[idx] = min(bucket_min[idx], num);
            bucket_max[idx] = max(bucket_max[idx], num);
        }
        
        int pre = 0, result = 0;
        
        for (int i = 1; i < cnt; i++) {
            if (bucket_min[i] == INT_MAX || bucket_max[i] == INT_MIN) {
                continue;
            }
            
            result = max(result, bucket_min[i] - bucket_max[pre]);
            pre = i;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题借用了桶排序的思路，整体来说还是比较难的，严格的证明我也还没搞懂。这道题目的分享到这里，感谢你的支持！
