---
title: LeetCode解题报告（240）-- 941. Valid Mountain Array
tags:
  - LeetCode
mathjax: true
date: 2020-12-10 17:37:24


---

## Problem

Given an array of integers `arr`, return *true if and only if it is a valid mountain array*.

Recall that arr is a mountain array if and only if:

- `arr.length >= 3`
- There exists some`i`with`0 < i < arr.length - 1`such that:
  - `arr[0] < arr[1] < ... < arr[i - 1] < A[i]`
  - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

<!-- more -->

![Example](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

**Example 1:**

```
Input: arr = [2,1]
Output: false
```

**Examplee 2:**

```
Input: arr = [3,5,5]
Output: false
```

**Example 3:**

```
Input: arr = [0,3,2,1]
Output: true
```

**Constraints:**

- `1 <= arr.length <= 104`
- `0 <= arr[i] <= 104`

------

## Analysis

&emsp;&emsp;题目比较简单，要求判断一个数组是否mountain array。要求数组中数字的值是**先严格递增再严格递减**。$O(n)$内完成的思路有非常多，这里我分享一下我的思路。分别从两端出发，要求严格递增一直向中间走，直至不再满足。这个时候如果是满足题目要求的话，两个下标会聚在一起（山顶），通过这个判断就可以了。

------

## Solution

&emsp;&emsp;注意`i`遍历的范围是`[0, size - 2]`，因为`i`会往后看一个，所以会有越界的风险；同理，`j`遍历的范围是`[1, size - 1]`。

------

## Code

```c++
class Solution {
public:
    bool validMountainArray(vector<int>& arr) {
        int size = arr.size();
        int i = 0, j = size - 1;
        
        while (i < size - 1 && arr[i] < arr[i + 1]) {
            i++;
        }
        while (j > 0 && arr[j - 1] > arr[j]) {
            j--;
        }
        return i < size - 1 && j > 0 && i == j;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不高，在这里整理一下做题的思路。这道题目的分享到这里，谢谢您的支持！
