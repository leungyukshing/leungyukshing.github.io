---
title: LeetCode解题报告（131）-- 152. Maximum Product Subarray
tags:
  - LeetCode
mathjax: true
date: 2020-09-11 19:53:18

---

## Problem

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

<!-- more -->

**Example 1:**

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

------

## Analysis

&emsp;&emsp;这道题目是子序列问题，比较难的地方是要求乘积最大的。首先遇到这种子序列问题，还是求最大或最小的，毫无疑问优先考虑dp来做。然后我们来看如何能做到乘积最大。乘积最大一般有两种情况：

1. 正数乘一个很大的数（正数）；
2. 负数乘一个很小的数（负数）

&emsp;&emsp;所以在dp的过程中我们要维护两个状态，一个是最大值`f`，一个是最小值`g`。状态转移如下：

1. 最大值`f[i] = max(f[i - 1] * nums[i], g[i - 1] * nums[i], nums[i]) `；
2. 最小值`g[i] = max(f[i - 1] * nums[i], g[i - 1] * nums[i], nums[i])`

&emsp;&emsp;最大值和最小值产生的过程都是一样的，只不过一个取max一个取min，需要注意的是因为是子序列，所以也要把`nums[i]`考虑上，即前面的都不要，只考虑当前这个元素能获取最大的值。

&emsp;&emsp;而我们最终的结果并不是取`f` 或者`g`的最后一个元素，而是要取这个过程中能得到最大的值，所以用一个result变量来保存。

------

## Solution

&emsp;&emsp;初始化的时候记得把`result`初始化为第一个元素。

------

## Code

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int size = nums.size();
        vector<int> f(size, 0), g(size, 0);
        int result = 0;
        
        result = f[0] = g[0] = nums[0];
        for (int i = 1; i < size; i++) {
            f[i] = max(max(f[i - 1] * nums[i], g[i - 1] * nums[i]), nums[i]);
            g[i] = min(min(f[i - 1] * nums[i], g[i - 1] * nums[i]), nums[i]);
            result = max(result, f[i]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;本质上还是很经典的dp问题，只不过从加法的最大最小值问题转化为乘法，这道题目的关键是理解乘法如何去得到一个比较大的数。这道题的分析到这里，谢谢！
