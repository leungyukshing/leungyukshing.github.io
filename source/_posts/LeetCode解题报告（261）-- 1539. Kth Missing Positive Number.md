---
title: LeetCode解题报告（261）-- 1539. Kth Missing Positive Number
tags:
  - LeetCode
mathjax: true
date: 2021-01-09 00:40:02


---

## Problem

Given an array `arr` of positive integers sorted in a **strictly increasing order**, and an integer `k`.

*Find the* `kth` *positive integer that is missing from this array.*

<!-- more -->

**Example 1:**

```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```

**Example 2:**

```
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation: The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```

**Constraints:**

- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
- `1 <= k <= 1000`
- `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

------

## Analysis

&emsp;&emsp;这道题目给出一个数组和一个数`K`，要求找出缺失的第K个数字。首先题目给出的数组是严格递增的，我们可以很好地利用这个特性。我们先来看数字和下标的关系，正常情况下，如果没有缺失的话，应该是有`arr[i] = i + 1`。所以如果我们要找出缺失的数字，就要先计算出值和下标的差距。以Example1为例，我们计算出每个数字和下标的差`[1, 1, 1, 3, 6]`。当值为7的时候，差是3，即缺失的数字的长度是3，是`[1,5,6]`；当值是11时，差是6，即缺失的数字的长度是6，是`[1,5,6,8,9,10]`，这个时候就超过了`K`。到这一步，我们知道如何找到在哪个位置缺失超过了`K`个数字。

&emsp;&emsp;知道了缺失了超过`K`个数字，我们就要精确定位到缺失的第`K`个数字是什么。这个其实非常简单，用该数字的下标加上`K`即可。

------

## Solution

&emsp;&emsp;如何找到在哪个位置缺失超过了`K `个数字可以用线性的方法，复杂度就是`O(n)`，但其实这里本质上是对`diff`去做搜索，所以可以用二分，把复杂度进一步降到`O(logn)`。

------

## Code

```c++
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        int left = 0;
        int right = arr.size();
        while (left < right) {
            int mid = (left + right) / 2;
            if (arr[mid] - mid >= k + 1) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left + k;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目看上去比较简单，但实际上解决起来还是比较巧妙，借助了下标的方法去判断。同时使用二分法去降低计算复杂度，所以这道题是非常适合作为面试题的。这道题这道题目的分享到这里，谢谢您的支持！
