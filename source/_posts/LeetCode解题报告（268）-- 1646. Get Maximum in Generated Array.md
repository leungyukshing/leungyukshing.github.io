---
title: LeetCode解题报告（268）-- 1646. Get Maximum in Generated Array
tags:
  - LeetCode
mathjax: true
abbrlink: 65194
date: 2021-01-15 22:33:51
---

## Problem

You are given an integer `n`. An array `nums` of length `n + 1` is generated in the following way:

- `nums[0] = 0`
- `nums[1] = 1`
- `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
- `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

Return *the **maximum** integer in the array* `nums`.

<!-- more -->

**Example 1:**

```
Input: n = 7
Output: 3
Explanation: According to the given rules:
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
Hence, nums = [0,1,1,2,1,3,2,3], and the maximum is 3.
```

**Example 2:**

```
Input: n = 2
Output: 1
Explanation: According to the given rules, the maximum between nums[0], nums[1], and nums[2] is 1.
```

**Example 3:**

```
Input: n = 3
Output: 2
Explanation: According to the given rules, the maximum between nums[0], nums[1], nums[2], and nums[3] is 2.
```

**Constraints:**

- `0 <= n <= 100`

------

## Analysis

&emsp;&emsp;题目给出了一个数`n`，要求我们按照题目给定的规则生成一个长度为`n + 1`的数组，最后找出这个数组中最大的数。我们重点来看题目给出的规则，前两个是初始化项，着重关注后面两个：

- `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`：对于这条规则，实际上是指下标为偶数的项，同时根据范围约束可以知道，它针对的是所有的偶数下标，除了0；
- `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`：那这条规则显然就是指下标为奇数的项，同时根据范围约束可以知道，它针对的是所有的奇数下标，除了1。

&emsp;&emsp;所以题目给出的规则定义实际上分了两类，奇数和偶数，接下来我们只需要做一个简单的转换即可。以下标为偶数的情况为例，令`2 * i = j`，则`i =j / 2`，代入到题目的条件就是：`nums[j] = nums[j / 2]`；同样地，下标为奇数的情况也可以转换为：`nums[j] = nums[j / 2 - 1/ 2] + nums[j / 2 + 1 / 2]`。这种情况因为`j`是奇数，所以`j / 2 - 1 / 2`实际上就是`j / 2`，而`j / 2 + 1 / 2`实际上就是`j / 2 + 1`。这样就得到了我们的递推公式了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int getMaximumGenerated(int n) {
        if (n == 0) {
            return 0;
        }
        vector<int> nums(n + 1);
        nums[0] = 0;
        nums[1] = 1;
        for (int i = 2; i <= n; i++) {
            if (i % 2 == 0) {
                nums[i] = nums[i / 2];
            } else {
                nums[i] = nums[i / 2] + nums[i / 2 + 1];
            }
        }
        return *max_element(nums.begin(), nums.end());
    }
};
```

------

## Summary

&emsp;&emsp;这道题目比较简单，主要就是利用了数学的换元思路，统一处理了下标。这道题这道题目的分享到这里，谢谢您的支持！
