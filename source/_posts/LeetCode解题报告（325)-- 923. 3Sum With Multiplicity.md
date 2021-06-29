---
title: LeetCode解题报告（325)-- 923. 3Sum With Multiplicity
tags:
  - LeetCode
mathjax: true
abbrlink: 51861
date: 2021-03-30 01:52:11
---

## Problem

Given an integer array `arr`, and an integer `target`, return the number of tuples `i, j, k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it **modulo** `109 + 7`.

<!-- more -->

**Example 1:**

```
Input: arr = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation: 
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.
```

**Example 2:**

```
Input: arr = [1,1,2,2,2,2], target = 5
Output: 12
Explanation: 
arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:
We choose one 1 from [1,1] in 2 ways,
and two 2s from [2,2,2,2] in 6 ways.
```

**Constraints:**

- `3 <= arr.length <= 3000`
- `0 <= arr[i] <= 100`
- `0 <= target <= 300`

------

## Analysis

&emsp;&emsp;题目是3 sum的改进版，最原始的3 sum的数组中的数字是唯一的，而这里数组中的数字是不唯一的，题目要求找出能够构成目标值的三个数字的组合的数量。所以这道题目可以分成两个部分，第一个是找出三个数加起来为目标值，第二个是把组合的数量计算出来。

&emsp;&emsp;第一部分好做，就是3 sum的做法。先确定一个数组，然后有左右两个下标来找出剩下的两个数，这个比较简单，这里不再赘述。

&emsp;&emsp;难的是第二部分，如何求出满足条件的组合数量。如果不存在重复的数字，那么找到的满足题目条件的三元组数量就是答案的数量。这里要解决的是重复数字的问题，其实也并不难。因为我们在处理第一部分的时候，已经把数组排好序，所以重复的数字是相邻出现的，因此在找到剩下的两个数字后（分别是`arr[j]`和`arr[k]`），我们找与这两个数字相同的数分别有多少个。`j`往右找，假设找到了`left`个；`k`往左找，假设找到了`right`个。这样就知道每个数字各有多少个，相互组合即可。

------

## Solution

&emsp;&emsp;这里有个点需要注意，找到重复数组组合时，要分情况处理：

1. 当`arr[j] != arr[k]`时，组合数量就是$left \times right$；
2. 当`arr[j] == arr[k]`时，说明这两个数字是一样的，所以`k - j`就是这个数字出现的数量，而计算组合时，我们需要从中抽取两个出来，利用组合数公式，此时组合的数量就是$\frac{(k - j + 1) \times (k - j)}{2}$。

------

## Code

```c++
class Solution {
public:
    int threeSumMulti(vector<int>& arr, int target) {
        long result = 0, size = arr.size(), M = 1e9 + 7;
        sort(arr.begin(), arr.end());
        for (int i = 0; i < size - 2; i++) {
            int sum = target - arr[i];
            
            int j = i + 1, k = size - 1; // search window[i + 1, size - 1]
            while (j < k) {
               if (arr[j] + arr[k] < sum) {
                   j++;
               } else if (arr[j] + arr[k] > sum) {
                   k--;
               } else {
                   int left = 1, right = 1;
                   while (j + left < k && arr[j + left] == arr[j]) {
                       left++;
                   }
                   while (j + left <= k - right && arr[k - right] == arr[k]) {
                       right++;
                   }
                   result += arr[j] == arr[k] ? (k - j + 1) * (k - j) / 2: left * right;
                   j += left;
                   k -= right;
               }
            }
        }
        return result % M;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是属于3sum的一个变种，找三元组的方法是一样的，增加的难点在于如何处理重复数字带来的组合问题。同时还要特别注意相同数组的组合计算问题。这道题目的分享到这里，感谢你的支持！
