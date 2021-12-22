---
title: LeetCode解题报告（433）-- 2100. Find Good Days to Rob the Bank
mathjax: true
date: 2021-12-22 12:38:25
---

## Problem

You and a gang of thieves are planning on robbing a bank. You are given a **0-indexed** integer array `security`, where `security[i]` is the number of guards on duty on the `ith` day. The days are numbered starting from `0`. You are also given an integer `time`.

The `ith` day is a good day to rob the bank if:

- There are at least `time` days before and after the `ith` day,
- The number of guards at the bank for the `time` days **before** `i` are **non-increasing**, and
- The number of guards at the bank for the `time` days **after** `i` are **non-decreasing**.

More formally, this means day `i` is a good day to rob the bank if and only if `security[i - time] >= security[i - time + 1] >= ... >= security[i] <= ... <= security[i + time - 1] <= security[i + time]`.

Return *a list of **all** days **(0-indexed)** that are good days to rob the bank*. *The order that the days are returned in does **not** matter.*

<!-- more -->

**Example 1:**

```
Input: security = [5,3,3,3,5,6,2], time = 2
Output: [2,3]
Explanation:
On day 2, we have security[0] >= security[1] >= security[2] <= security[3] <= security[4].
On day 3, we have security[1] >= security[2] >= security[3] <= security[4] <= security[5].
No other days satisfy this condition, so days 2 and 3 are the only good days to rob the bank.
```

**Example 2:**

```
Input: security = [1,1,1,1,1], time = 0
Output: [0,1,2,3,4]
Explanation:
Since time equals 0, every day is a good day to rob the bank, so return every day.
```

**Example 3:**

```
Input: security = [1,2,3,4,5,6], time = 2
Output: []
Explanation:
No day has 2 days before it that have a non-increasing number of guards.
Thus, no day is a good day to rob the bank, so return an empty list.
```

**Example 4:**

```
Input: security = [1], time = 5
Output: []
Explanation:
No day has 5 days before and after it.
Thus, no day is a good day to rob the bank, so return an empty list.
```

**Constraints:**

- `1 <= security.length <= 105`
- `0 <= security[i], time <= 105`

------

## Analysis

&emsp;&emsp;又是抢劫银行的题目，但这个不是打家劫舍的系列题了，所以并不是无脑的dp。先来看看题目要求，题目给出一个数组`security`，定义good day为某天的前`time`天都是非递增的，后`time`天都是非递减的，问数组中所有的good day。首先看最直接的做法就是遍历一遍数组，对每个位置检查一下前`time`天和后`time`天是否满足题目的要求。但这里的问题是有很多重复的计算，比如处理第`i`天时，已经把前面计算了一遍，当处理第`i + 1`天时，又需要把前面的再处理一边，这就是需要优化的点。

&emsp;&emsp;回顾前面[1525. Number of Good Ways to Split a String](https://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88430%EF%BC%89--%201525.%20Number%20of%20Good%20Ways%20to%20Split%20a%20String.html#more)的优化思路，两道题其实本质是一样的，都要进行预先计算从而减少重复计算。这里提前计算也是有两部分，一个是从左到右非递增的长度，另一个是从右到左非递归的长度，注意单独一个数字也是一个序列。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> goodDaysToRobBank(vector<int>& security, int time) {
        int size = security.size();
        vector<int> before(size, 0), after(size, 0);
        
        for (int i = 1; i < size; ++i) {
            if (security[i] <= security[i - 1]) {
                before[i] = before[i - 1] + 1;
            }
        }
        
        for (int i = size - 2; i >= 0; --i) {
            if (security[i] <= security[i + 1]) {
                after[i] = after[i + 1] + 1;
            }
        }
        
        vector<int> result;
        for (int i = 0; i < size; i++) {
            if (i - time < 0 || i + time > size) {
                continue;
            }
            
            if (before[i] >= time && after[i] >= time) {
                result.push_back(i);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目相当于又给我们复习了通过提前计算减少重复运算的方法，这在处理数组类问题中非常常见。这道题目的分享到这里，感谢你的支持！
