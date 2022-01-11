---
title: LeetCode解题报告（507）-- 643. Maximum Average Subarray I
mathjax: true
date: 2022-01-10 22:28:13
---

## Problem

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return *this value*. Any answer with a calculation error less than `10-5` will be accepted.

<!-- more -->

**Example 1:**

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

**Example 2:**

```
Input: nums = [5], k = 1
Output: 5.00000
```



**Constraints:**

- `n == nums.length`
- `1 <= k <= n <= 105`
- `-104 <= nums[i] <= 104`

---

## Analysis

&emsp;&emsp;这道题目给出一个数组`nums`，要求找出长度为`k`的平均值最大的子数组，返回最大的平均值即可。首先是子数组问题，明确限定了长度为`k`，这两点是非常明显的sliding window提示。我们就按照sliding window的思路去做，然后这里要维护的是平均值最大，因为长度是固定的`k`，所以只需要维护数组的和最大。

&emsp;&emsp;如何快速求得数组的和呢？我们可以在sliding window的过程中逐一维护，但这里我用了preSum，更加快捷。最后只需要把最大的值除以`k`就能得到最大的平均数了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> preSum(n + 1, 0);
        for (int i = 1; i <= n; ++i) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        
        int left = 0, right = 0;
        int result = INT_MIN;
        while (right < n) {
            right++;
            while (right - left > k) {
                left++;
            }
            
            if (right - left == k) {
                result = max(result, preSum[right] - preSum[left]);
            }
        }
        return 1.0 * result / k;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目不难，sliding window的指向非常明显，同时也可以利用前缀和求区间的和，融合了两个知识点，我认为是一道很好的基础题。这道题目的分享到这里，感谢你的支持！

