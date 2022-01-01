---
title: LeetCode解题报告（487）-- 813. Largest Sum of Averages
mathjax: true
date: 2022-01-01 17:56:06
---

## Problem

You are given an integer array `nums` and an integer `k`. You can partition the array into **at most** `k` non-empty adjacent subarrays. The **score** of a partition is the sum of the averages of each subarray.

Note that the partition must use every integer in `nums`, and that the score is not necessarily an integer.

Return *the maximum **score** you can achieve of all the possible partitions*. Answers within `10-6` of the actual answer will be accepted.

<!-- more -->

**Example 1:**

```
Input: nums = [9,1,2,3,9], k = 3
Output: 20.00000
Explanation: 
The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned nums into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.
```

**Example 2:**

```
Input: nums = [1,2,3,4,5,6,7], k = 4
Output: 20.50000
```



**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 104`
- `1 <= k <= nums.length`

---

## Analysis

&emsp;&emsp;题目给出一个数组`nums`，还有一个`k`，问把数组至多分成`k`个subarray，使得每个subarray的平均值加起来最大是多少。这种一看是subarray又是求最值的问题，还是两个方向，sliding window或者dp。因为对于subarray没有长度的限制，也没有数值上的限制，所以sliding window并不是适用，于是我们就focus到dp上。数组类的dp一般有一个状态就是`i`，另一个状态就是`k`，但这里实际上是`k - 1`，因为分成`k`组就是做`k - 1`次分割。

&emsp;&emsp;先来看第一个状态`i`，一般我们会定义`i`是`[0, i]`这样，但是这道题目有点不一样，我们定义的是`[i, n-1]`，为什么要这样定义呢？因为这样用presum计算方便，假设我们要计算`dp[i][k]`,我们遍历`j`，`j`的范围是`[i + 1, n]`，那么`dp[i][k] = max(dp[i][k], average(i, j) + dp[j][k - 1])`。到这里我们其实已经可以写出转移方程了。

&emsp;&emsp;然后来看两个可优化的部分，第一个部分是求平均数，前面提到用presum计算，所以我们可以先计算出一个presum的数组，求`average(i, j) = (preSum[j] - preSum[i]) / (j - i)`（注意这个是不包含`nums[j]`的）。第二个部分是dp方程，可以看出我们每次都需要计算完所有的`k - 1`，再计算`k`，所以我们只用一个状态就可以了，可以把`k`这个状态省略掉。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    double largestSumOfAverages(vector<int>& nums, int K) {
        int size = nums.size();
        vector<int> preSum(size + 1, 0);
        for (int i = 1; i <= size; ++i) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
        
        vector<double> dp(size, 0.0);
        for (int i = 0; i < size; ++i) {
            dp[i] = (1.0 * (preSum[size] - preSum[i])) / (size - i);
        }
        
        for (int k = 0; k < K - 1; ++k) {
            for (int i = 0; i < size; ++i) {
                for (int j = i + 1; j < size; ++j) {
                    dp[i] = max(dp[i], dp[j] + (1.0 * (preSum[j] - preSum[i])) / (j - i));
                }
            }
        }
        return dp[0];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较常规的数组dp，里面包含了用presum计算平均数以及dp空间优化，是一道好题。在二维矩阵中使用并查集要注意下标转换，同时还要特别注意初始化的时机。这道题目的分享到这里，感谢你的支持！

