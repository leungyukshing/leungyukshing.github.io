---
title: LeetCode解题报告（465）-- 1588. Sum of All Odd Length Subarrays
mathjax: true
date: 2021-12-27 21:59:01
---

## Problem

Given an array of positive integers `arr`, calculate the sum of all possible odd-length subarrays.

A subarray is a contiguous subsequence of the array.

Return *the sum of all odd-length subarrays of* `arr`.

<!-- more -->

**Example 1:**

```
Input: arr = [1,4,2,5,3]
Output: 58
Explanation: The odd-length subarrays of arr and their sums are:
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
If we add all these together we get 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58
```

**Example 2:**

```
Input: arr = [1,2]
Output: 3
Explanation: There are only 2 subarrays of odd length, [1] and [2]. Their sum is 3.
```

**Example 3:**

```
Input: arr = [10,11,12]
Output: 66
```



**Constraints:**

- `1 <= arr.length <= 100`
- `1 <= arr[i] <= 1000`

---

## Analysis

&emsp;&emsp;题目给出一个数组`arr`，要计算出所有长度为奇数的subarray的和。因为这是一道easy题，可以用暴力的解法，穷举出所有的子数组然后都加起来得到答案。当然了我能写题解肯定是因为有更好的解法，直接$O(n)$时间复杂度解决。

&emsp;&emsp;因为这里是把子数组的数字加起来，那么我们先找一下规律，看一下每个数字出现在奇数长度的子数组的次数，乘上本身不就是答案了吗？所以问题就转化为对于某个数`arr[i]`，它一共在多少个奇数长度的子数组出现过？我们就随便找个`i`的情况分析下：

+ subarray的开头可以是0、1、2、3、i，所以一共有`i + 1`种情况；
+ subarray的结尾可以是`i + 1`、`i + 2`、...、`n`，一共有`n - 1`种情况。

&emsp;&emsp;注意，这里我们讨论的是包含`arr[i]`的子数组个数，因为我们要算长度为奇数的，所以除以2。之后的计算就是乘上`arr[i]`本身加到结果中即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int sumOddLengthSubarrays(vector<int>& arr) {
        int result = 0;
        int n = arr.size();
        for (int i = 0; i < n; ++i) {
            int contribution = ((n - i) * (i + 1) + 1) / 2;
            result += contribution * arr[i];
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的$O(n)$解法还是比较有难度的，这道题目也给我们提供了一个在数组中找出每个元素出现在子数组的次数这样一个思路。这道题目的分享到这里，感谢你的支持！
