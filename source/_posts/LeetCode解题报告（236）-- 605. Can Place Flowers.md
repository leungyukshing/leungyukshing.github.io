---
title: LeetCode解题报告（236）-- 605. Can Place Flowers
tags:
  - LeetCode
mathjax: true
abbrlink: 63165
date: 2020-12-06 01:01:30
---

## Problem

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return *if* `n` new flowers can be planted in the `flowerbed` without violating the no-adjacent-flowers rule.

<!-- more -->

**Example 1:**

```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
```

**Example 2:**

```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
```

**Constraints:**

- `1 <= flowerbed.length <= 2 * 104`
- `flowerbed[i]` is `0` or `1`.
- There are no two adjacent flowers in `flowerbed`.
- `0 <= n <= flowerbed.length`

------

## Analysis

&emsp;&emsp;这道题给定了一个只包含01的数组，0代表空（可以放置），1代表已经占有（不可放置），然后再给出了待放置的花的数量`n`，要求计算出，在花之间不能相邻的情况下，能不能把这些花都放进去。这个和House Robber系列的题目还有点不一样，因为题目给出的了数组中包含了1，所以并不是单纯地dp。

&emsp;&emsp;先来看最简单的一种情况，三个连续空位`000`。按照题目的要求有两种放的方法，一是`101`，二是`010`。第一种方式的前提是前面和后面都是0（即必须是`00000`->`01010`），所以在头尾的情况要考虑清楚。这里可以做一个转化，把第一种情况统一到第二种中处理。做法是在首尾都加一个0，变成`0000000`，在这种情况下放置的情况就是`0101010`，这样就兼容了第二种的做法。

&emsp;&emsp;按照上面第二种的计算方式，如果有连续`k`个空闲位置，那么中间最多可以放置花的数量是$(k - 1) /2$，按照这个公式，从头到尾遍历数组，找到连续的`k`个空闲位置后就计算一下，最后统计出这个数组最多还能够放多少枝花，再和题目给出的`n`相比即可。

------

## Solution

&emsp;&emsp;遍历的时候下标是$[0, size]$，这是因为兼容处理最后一个位置是空闲位置的情况。如果最后一个位置也是空闲的话，当结束的时候还是需要计算一波。

------

## Code

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        if (flowerbed.size() == 0) {
            return false;
        }
        if (flowerbed[0] == 0) {
            flowerbed.insert(flowerbed.begin(), 0);
        }
        if (flowerbed.back() == 0) {
            flowerbed.push_back(0);
        }
        
        int size = flowerbed.size(), k = 0, sum = 0;
        for (int i = 0; i <= size; i++) {
            if (i < size && flowerbed[i] == 0) {
                k++;
            } else {
                sum += (k - 1) / 2;
                k = 0;
            }
        }
        return sum >= n;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目在算法层面上没有过多的内容，难度主要是把不同的情况整合起来。想这类题目我们不妨先找一个最简单的case去分析，然后通过冗余的方式去统一处理逻辑。这道题目的分享到这里，谢谢您的支持！
