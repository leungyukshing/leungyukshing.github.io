---
title: LeetCode解题报告（73）-- 1217. Play with Chips
tags:
  - LeetCode
abbrlink: 10803
date: 2019-10-26 14:00:07
---

## Problem

There are some chips, and the i-th chip is at position `chips[i]`.

You can perform any of the two following types of moves **any number of times** (possibly zero) **on any chip**:

- Move the `i`-th chip by 2 units to the left or to the right with a cost of **0**.
- Move the `i`-th chip by 1 unit to the left or to the right with a cost of **1**.

There can be two or more chips at the same position initially.

Return the minimum cost needed to move all the chips to the same position (any position).

<!-- more -->

**Example 1:**

```
Input: chips = [1,2,3]
Output: 1
Explanation: Second chip will be moved to positon 3 with cost 1. First chip will be moved to position 3 with cost 0. Total cost is 1.
```

**Example 2:**

```
Input: chips = [2,2,2,3,3]
Output: 2
Explanation: Both fourth and fifth chip will be moved to position two with cost 1. Total minimum cost will be 2.
```

**Constraints:**

- `1 <= chips.length <= 100`
- `1 <= chips[i] <= 10^9`

------

## Analysis

&emsp;&emsp;这道题和数组有关，题目给出每个chip所在的位置，同时给出移动每一个chip的代价，要求我们花费最小的cost将所有的chip移动到同一个位置。第一眼看题目觉得情况有很多种，比较复杂。我们不妨从结果倒推回去。因为最终所有的chip要求在同一个位置，又因为根据移动的规则，移动2格是不消费cost的，因此可以理解为：与最终位置奇偶性相同的位置上的chip移动并不需要花费cost。即如果最后chip都移动到下标为2的位置，那么下标为偶数的位置移动到该最终位置都不需要消耗cost，而下标为奇数的位置的cost都需要1。

&emsp;&emsp; 分析到这里，我们知道了cost最小的情况就是看下标为奇数的多还是下标为偶数的多，较多的那个作为最终位置，较小的那个就是我们要求的cost了。

------

## Solution

&emsp;&emsp;遍历一遍计算下标的奇偶性即可。

------

## Code

```c++
class Solution {
public:
    int minCostToMoveChips(vector<int>& chips) {
        int size = chips.size();
        int even = 0;
        for (int i = 0; i < size; i++) {
            if (chips[i] % 2 == 0) {
                even++;
            }
        }
        return min(even, size - even);
    }
};
```

------

## Summary

 &emsp;&emsp;这道题给我们提供了一种思路，在面对数组类型的题目时，很多时候与奇偶性有关，所以我们在做这类题目的时候需要有一定的敏感。这道题的分享到这里，谢谢您的支持！
