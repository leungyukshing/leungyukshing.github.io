---
title: LeetCode解题报告（283）-- 1675. Minimize Deviation in Array
tags:
  - LeetCode
mathjax: true
abbrlink: 60459
date: 2021-02-09 13:39:43
---

## Problem

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

- If the element is **even**, **divide** it by `2`.
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
- If the element is **odd**, **multiply** it by `2`.
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The **deviation** of the array is the **maximum difference** between any two elements in the array.

Return *the **minimum deviation** the array can have after performing some number of operations.*

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.
```

**Example 2:**

```
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.
```

**Example 3:**

```
Input: nums = [2,10,8]
Output: 3
```

**Constraints:**

- `n == nums.length`
- `2 <= n <= 105`
- `1 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组，允许对数组进行两种操作，使得数组的极差最小。两种操作分别是：

1. 对于偶数，可以除以2；
2. 对于奇数，可以乘以2；

&emsp;&emsp;同时处理两种操作比较复杂，所以我们可以将两种操作统一成一种。首先把所有的奇数都乘以2，这样变化后的数组就只有偶数了，所以就只有除以2的操作。从直观上看，想要得到较小的极差，大的数字要尽量小，所以我们每次都拿最大的数，如果这个数是奇数，说明这个就是它原来的值，没有办法再变化，这个时候就可以退出循环；如果是偶数就除以2，继续循环。整个过程维护一个最优解即可。

------

## Solution

&emsp;&emsp;从上面的分析可以知道，我们需要每次都拿最大的数，所以这里又用到了优先队列。

------

## Code

```c++
class Solution {
public:
    int minimumDeviation(vector<int>& nums) {
        set<int> s;
        for (int x : nums)
          s.insert(x & 1 ? x * 2 : x);
        int ans = *rbegin(s) - *begin(s);
        while (*rbegin(s) % 2 == 0) {
          s.insert(*rbegin(s) / 2);
          s.erase(*rbegin(s));
          ans = min(ans, *rbegin(s) - *begin(s));
        }
        return ans;
    }
};
```

------

## Summary

&emsp;&emsp;这道题难度是hard，主要体现在思考上。首先是把两种操作统一成一种操作，然后是利用优先队列解决问题，非常考验综合能力。这道题这道题目的分享到这里，谢谢您的支持！
