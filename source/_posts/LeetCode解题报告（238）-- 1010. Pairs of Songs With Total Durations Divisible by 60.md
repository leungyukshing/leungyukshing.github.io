---
title: LeetCode解题报告（238）-- 1010. Pairs of Songs With Total Durations Divisible by 60
tags:
  - LeetCode
mathjax: true
abbrlink: 52701
date: 2020-12-08 20:12:29
---

## Problem

You are given a list of songs where the ith song has a duration of `time[i]` seconds.

Return *the number of pairs of songs for which their total duration in seconds is divisible by* `60`. Formally, we want the number of indices `i`, `j` such that `i < j` with `(time[i] + time[j]) % 60 == 0`.

<!-- more -->

**Example 1:**

```
Input: time = [30,20,150,100,40]
Output: 3
Explanation: Three pairs have a total duration divisible by 60:
(time[0] = 30, time[2] = 150): total duration 180
(time[1] = 20, time[3] = 100): total duration 120
(time[1] = 20, time[4] = 40): total duration 60
```

**Example 2:**

```
Input: time = [60,60,60]
Output: 3
Explanation: All three pairs have a total duration of 120, which is divisible by 60.
```

**Constraints:**

- `1 <= time.length <= 6 * 104`
- `1 <= time[i] <= 500`

------

## Analysis

&emsp;&emsp;这道题给定一个数组，要求找到**和能够被60整除的数对**。在一个数组中找数对，求它们的和，这个大家应该不陌生吧，就是很经典的Two Sum，那这里能不能转化为类似的思路呢？要使得两个数的和能被60整除，实际上可以转化为**这两个数除以60的余数之和为60**。所以一下子这个题目就变得简单了，

&emsp;&emsp;我们首先计算所有数对60的余数，这样我们就把数量级从很大的数降到了60。然后考虑两种特殊的余数：0和30。如果两个数的余数都是0，那么它们之间任意组合的和都是60的倍数，所以这里组合的数量是$\frac{n \times (n-1)}{2}$；如果两个数的余数都是30，那么它们直接任意组合之后余数都会是0，也是60的倍数，所以组合的数量也同样是$\frac{n \times (n-1)}{2}$；剩下的就是要组合成60的Two Sum问题了。

------

## Solution

&emsp;&emsp;Two Sum的解题方法用到了map，这里我们也用一个map来存放余数和出现次数的对应关系即可。

------

## Code

```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        map<int, int> m;
        int size = time.size();
        for (int i = 0; i < size; i++) {
            m[time[i] % 60]++; 
        }
        
        int result = 0;
        map<int, int>::iterator it, it1;
        it = m.find(0);
        if (it != m.end()) {
            result += (m[0] * (m[0] - 1) / 2);
        }
        
        it = m.find(30);
        if (it != m.end()) {
            result += (m[30] * (m[30] - 1) / 2);
        }
        
        for (int i = 1; i < 30; i++) {
            it = m.find(i);
            it1 = m.find(60 - i);
            if (it != m.end() && it1 != m.end()) {
                result += m[i] * m[60 - i];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目从题面上看比较新鲜，但实际上却是经典题目Two Sum的变种。通过这道题目，也能反过头来理解Two Sum的解题思路。这道题目的分享到这里，谢谢您的支持！
