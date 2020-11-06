---
title: LeetCode解题报告（209）-- 1283. Find the Smallest Divisor Given a Threshold
tags:
  - LeetCode
mathjax: true
date: 2020-11-07 01:05:20

---

## Problem

Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer divisor and divide all the array by it and sum the result of the division. Find the **smallest** divisor such that the result mentioned above is less than or equal to `threshold`.

Each result of division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).

It is guaranteed that there will be an answer.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,5,9], threshold = 6
Output: 5
Explanation: We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum to 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2). 
```

**Example 2:**

```
Input: nums = [2,3,5,7,11], threshold = 11
Output: 3
```

**Example 3:**

```
Input: nums = [19], threshold = 5
Output: 4
```

**Constraints:**

- `1 <= nums.length <= 5 * 10^4`
- `1 <= nums[i] <= 10^6`
- `nums.length <= threshold <= 10^6`

------

## Analysis

&emsp;&emsp;这道题目要求我们找出一个最大的divisor，使得数组中每个元素除以这个divisor之后剩下结果之和最大，但是要小于`threshold`。很直接的思路就是每一个divisor都试一下，计算一下结果，保留最大且不超过`threshold`的就可以了。divisor的范围也不难确定，因为这道题目中的除法是**向上取整**，所以当divisor超过了是这个数组中最大的元素后，他的结果都是一样的，因此这个divisor的取值是有范围的，和`nums[i]`的取值范围一致。

&emsp;&emsp;这种做法听上去就有点无脑，复杂度可能不是这么好。这里就要介绍一种做题的思路**二分答案**。这道题目的最终答案是除以divisor结果之和，但实际上决定这个和的还是divisor本身，所以可以把divisor视作是答案。根据上面的分析我们可以知道，divisor是有范围的，所以我们可以通过在这个区间中通过二分，找到满足题目条件的答案。

------

## Solution

&emsp;&emsp;二分答案使用的也是常规的二分法，没有太大难度。C++中向上取整可以用`ceil()`。

------

## Code

```c++
class Solution {
public:
    int smallestDivisor(vector<int>& nums, int threshold) {
        int left = 1, right = 1000000;
        int size = nums.size();
        while (left < right) {
            int mid = (left + right) / 2;
            int sum = 0;
            for (int i = 0; i < size; i++) {
                sum += ceil(1.0 * nums[i] / mid);
            }
            if (sum > threshold) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return right;
    }
};
```

------

## Summary

&emsp;&emsp;从这道题目引申出一种解题方法——**二分答案**。其思路就是在答案区间已知的情况下，通过对答案区间的二分，找到满足题目要求的最优答案。这道题目的分享到这里，谢谢！
