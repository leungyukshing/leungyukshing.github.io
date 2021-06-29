---
title: LeetCode解题报告（337) -- 775. Global and Local Inversions
tags:
  - LeetCode
mathjax: true
abbrlink: 7136
date: 2021-04-11 01:50:12
---

## Problem

You are given an integer array `nums` of length `n` which represents a permutation of all the integers in the range `[0, n - 1]`.

The number of **global inversions** is the number of the different pairs `(i, j)` where:

- `0 <= i < j < n`
- `nums[i] > nums[j]`

The number of **local inversions** is the number of indices `i` where:

- `0 <= i < n - 1`
- `nums[i] > nums[i + 1]`

Return `true` *if the number of **global inversions** is equal to the number of **local inversions***.

<!-- more -->

**Example 1:**

```
Input: nums = [1,0,2]
Output: true
Explanation: There is 1 global inversion and 1 local inversion.
```

**Example 2:**

```
Input: nums = [1,2,0]
Output: false
Explanation: There are 2 global inversions and 1 local inversion.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5000`
- `0 <= nums[i] < n`
- All the integers of `nums` are **unique**.
- `nums` is a permutation of all the numbers in the range `[0, n - 1]`.

------

## Analysis

&emsp;&emsp;题目给出两个概念：局部倒序和全局倒序。局部倒序指的是**相邻**的的两个数字，下标较大的数值比较小；全局倒序指的是数组中任意的两个数字，下标较大的数值比较小。这两者的区别就在于，比较的这两个数字是否相邻。因为题目只需要判断局部倒序的数量是否等于全局倒序的数量，所以可以转化为，能否找到一个倒序，是全局倒序而不是局部倒序。如果能找到，则return false；否则return true。

&emsp;&emsp;我们先来看一个是全局倒序而不是局部倒序的例子，最简单的就是题目给出的Example2。`[1, 2, 0]`中，1在0的前面（而且不相邻），但是1比0要大。所以我们就是要找得到像这种结构，就说明存在全局倒序，而又不是局部倒序。接下来就看怎么去找这种结构。

&emsp;&emsp;暴力循环的方法这里就不提了，这里介绍一种比较巧妙的做法。正常来说，如果下标和值对应的话，下标为`i`的位置的值应该是`i + 1`，假设某个位置`i`的值不是`i + 1`，而是`i + 3`（因为如果是`i + 2`的话，就是局部倒序，这里要构造出全局倒序），那么说明`i + 2`的位置的值是`i + 1`。此时，位置`i`的值就是`i + 2`，所以下标和值之间差的绝对值大于1。我们就是要利用这个性质去寻找是否存在全局倒序。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool isIdealPermutation(vector<int>& A) {
        int size = A.size();
        for (int i = 0; i < size; i++) {
            if (abs(A[i] - i) > 1) {
                return false;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目重在分析，代码方面没有很复杂的逻辑和技术难点。这道题目的分享到这里，感谢你的支持！
