---
title: LeetCode解题报告（96）-- 264. Ugly Number II
tags:
  - LeetCode
date: 2020-07-04 17:34:50
mathjax: true

---

## Problem

Write a program to find the `n`-th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`. 

<!-- more -->

**Example 1:**

```
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

**Note:**  

1. `1` is typically treated as an ugly number.
2. `n` **does not exceed 1690**.

------

## Analysis

&emsp;&emsp;这道题目是和数学有关系的题目。题目对**ugly number**的定义是只能被2、3、5整除的正整数（其中1是特殊的）。题目要求我们返回第`n`大的ugly number。大致的思路有两种，一个生成一大堆ugly number，然后排序后找到第`n`大的；另一种就是直接从小到大地生成ugly number。

&emsp;&emsp;显然第二种是更加直观的方法，因为第一种方法我们没有办法控制要生成多少个ugly number才够，也没有办法保证大小关系。接下来问题就转化为如何**从小到大地生成ugly number**。根据ugly number的性质可以推断出，一个ugly number肯定是由更小的ugly number乘以2、3、5得到的，这也就是说ugly number的生成是循环的。以前几个ugly number数为例，1，2，3，4，5，6，那么4就是由2$\times$2得到的，而6则是由2$\times$3得到的。所以实际上我们生成有三个列表，分别是$\times$2，$\times$3和$\times$6。

|      | 1         | 2          | 3          | 4          | 5          | 6          | 8          |
| ---- | --------- | ---------- | ---------- | ---------- | ---------- | ---------- | ---------- |
| 2    | 1 * 2 = 2 | 2 * 2 = 4  | 3 * 2 = 6  | 4 * 2 = 8  | 5 * 2 = 10 | 6 * 2 = 12 | 8 * 2 = 16 |
| 3    | 1 * 3 = 3 | 2 * 3 = 6  | 3 * 3 = 9  | 4 * 3 = 12 | 5 * 3 = 15 | 6 * 3 = 18 | 8 * 3 = 24 |
| 5    | 1 * 5 = 5 | 2 * 5 = 10 | 3 * 5 = 15 | 4 * 5 = 20 | 5 * 5 = 25 | 6 * 5 = 30 | 8 * 5 = 40 |

&emsp;&emsp;由上面三个列表中就可以观察到ugly number的产生过程，但如何保证从小到大呢？实际上我们用三个下标表示每一个子列表进行到哪一步，然后每次同时计算三个数，最小的那个子列表下标增加。

------

## Solution

&emsp;&emsp;因为新的ugly number是从旧的产生，所以用一个vector去保存就ugly number就可以了，产生新的时候只会从之前的下标找。同时注意初始化第一个数为1，因为1是特殊的ugly number。

------

## Code

```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> res(1, 1);
        int i2 = 0, i3 = 0, i5 = 0;
        while (res.size() < n) {
            int m2 = res[i2] * 2, m3 = res[i3] * 3, m5 = res[i5] * 5;
            int mn = min(m2, min(m3, m5));
            if (mn == m2) ++i2;
            if (mn == m3) ++i3;
            if (mn == m5) ++i5;
            res.push_back(mn);
        }
        return res.back();
    }
};
```

------

## Summary

&emsp;&emsp;这道题是比较新颖的题目，重点是要观察出ugly number生成的方法和策略。然后使用下标来记录每个子列表的进度。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
