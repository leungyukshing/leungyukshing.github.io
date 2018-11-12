---
title: LeetCode解题报告（30）-- 908. Smallest Range I
tags:
  - LeetCode
abbrlink: 49713
date: 2018-11-06 15:44:22
---
## Problem
Given an array `A` of integers, for each integer `A[i]` we may choose any x with `-K <= x <= K`, and add `x` to `A[i]`.

After this process, we have some array `B`.

Return the smallest possible difference between the maximum value of `B` and the minimum value of `B`.
<!-- more -->

**Example 1**:
```
Input: A = [1], K = 0
Output: 0
Explanation: B = [1]
```

**Example 2**:
```
Input: A = [0,10], K = 2
Output: 6
Explanation: B = [2,8]
```

**Example 3**:
```
Input: A = [1,3,6], K = 3
Output: 0
Explanation: B = [3,3,3] or B = [4,4,4]
```

**Note:**
  1. `1 <= A.length <= 10000`
  2. `0 <= A[i] <= 10000`
  3. `0 <= K <= 10000`

## Analysis
&emsp;&emsp;这道题目给定了一个数组`A`和`K`，要求我们在`-K <= x <= K`中找到一个`x`，使得`B[i] = x + A[i]`（不同`i`可以对应不同`x`），然后找出B中最大值和最小值的差。于是很自然地想到，对`A`先进行排序，然后**最大值减去-K，最小值加上K**，从直观上将，这会使得最大的数最大程度地减小，使得最小的数最大程度地增加，他们之间的差就是最小的。
&emsp;&emsp;提交代码后发现有错，错误的原因是，如果处理后，`最大值减去K`<`最小值加上K`，那么他们的差就是一个负数，这是错误的。因为这个时候`最大值减去K`就不是`B`中的最大值了。但不用灰心，我们总的思路是对的，这里只需要对他们的差进行判断。如果差小于0，说明`A最大值减去K`之后就比`A最小值加上K`小，也就是说，我们能够找到一个`x`，`x < K`，使得`A最大值减去x`==`A最小值加上x`，这里`x`可能为小数。

## Solution
&emsp;&emsp;按照上面的方法编程即可，没有特别难的地方。

## Code
```C++
class Solution {
public:
    int smallestRangeI(vector<int>& A, int K) {
        sort(A.begin(), A.end());
        int diff = (A[A.size() - 1] - K) - (A[0] + K);
        if (diff < 0) {
            return 0;
        }
        return diff;
    }
};
```
**运行时间：**约20ms，超过46.24%的CPP代码。

## Summary
&emsp;&emsp;这道题的难点在于对`diff<0`的处理，需要在纸上自己证明一下`x`的存在性，其他就没什么难度了。本题的题析到此为止，谢谢您的支持！
