---
title: LeetCode解题报告（338) -- 1551. Minimum Operations to Make Array Equal
tags:
  - LeetCode
mathjax: true
date: 2021-04-11 10:21:36

---

## Problem

You have an array `arr` of length `n` where `arr[i] = (2 * i) + 1` for all valid values of `i` (i.e. `0 <= i < n`).

In one operation, you can select two indices `x` and `y` where `0 <= x, y < n` and subtract `1` from `arr[x]` and add `1` to `arr[y]` (i.e. perform `arr[x] -=1 `and `arr[y] += 1`). The goal is to make all the elements of the array **equal**. It is **guaranteed** that all the elements of the array can be made equal using some operations.

Given an integer `n`, the length of the array. Return *the minimum number of operations* needed to make all the elements of arr equal.

<!-- more -->

**Example 1:**

```
Input: n = 3
Output: 2
Explanation: arr = [1, 3, 5]
First operation choose x = 2 and y = 0, this leads arr to be [2, 3, 4]
In the second operation choose x = 2 and y = 0 again, thus arr = [3, 3, 3].
```

**Example 2:**

```
Input: n = 6
Output: 9
```

**Constraints:**

- `1 <= n <= 10^4`

------

## Analysis

&emsp;&emsp;题目给出一个数`n`，数组中从1开始，包含`n`个奇数。然后定义一次操作为：对下标`x`、`y`，`arr[x] += 1`，`arr[y] -= 1`。实际上就是选两个位置，一个加一，一个减一。问最少多少次操作后，能够使数组中所有的数都相等。

&emsp;&emsp;首先我们要处理的数组是等差数列，在等差数列中要把每个数都操作成相等，所以如果要数组中的值都变成一样，目标值应该就是平均值了。这样一来，每次操作都是头尾对应，第一个数和倒数第一个数对应，第二个数和倒数第二个数对应，所以我们只需要计算一半就可以了。

------

## Solution

&emsp;&emsp;计算目标值不需要整个数组遍历，可以直接用等差数列求和公式算。然后也不需要申请数组空间，用循环的变量去计算每一个位置和平均值的差，就是操作的次数了。

------

## Code

```c++
class Solution {
public:
    int minOperations(int n) {
        int sum = n * n;
        int target = sum / n;
        int result = 0;
        // cout << target << endl;
        for (int i = 0; i < n / 2; i++) {
            result += abs((i * 2 + 1) - target);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不算大，偏数学方面的知识多一些，coding上没有特别难的地方。这道题目的分享到这里，感谢你的支持！
