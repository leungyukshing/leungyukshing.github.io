---
title: LeetCode解题报告（113）-- 983. Minimum Cost For Tickets
tags:
  - LeetCode
mathjax: true
abbrlink: 12337
date: 2020-08-26 12:28:19
---

## Problem

In a country popular for train travel, you have planned some train travelling one year in advance.  The days of the year that you will travel is given as an array `days`.  Each day is an integer from `1` to `365`.

Train tickets are sold in 3 different ways:

- a 1-day pass is sold for `costs[0]` dollars;
- a 7-day pass is sold for `costs[1]` dollars;
- a 30-day pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.  For example, if we get a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7, and 8.

Return the minimum number of dollars you need to travel every day in the given list of `days`.

<!-- more -->

**Example 1:**

```
Input: days = [1,4,6,7,8,20], costs = [2,7,15]
Output: 11
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
In total you spent $11 and covered all the days of your travel.
```

**Example 2:**

```
Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
Output: 17
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
In total you spent $17 and covered all the days of your travel.
```

**Note:**

1. `1 <= days.length <= 365`
2. `1 <= days[i] <= 365`
3. `days` is in strictly increasing order.
4. `costs.length == 3`
5. `1 <= costs[i] <= 1000`

------

## Analysis

&emsp;&emsp;这道题一看就是用动态规划了。因为要求的是最少的价钱，后面买不买是取决于前面的决定，这就是典型的动态规划。状态转移主要有两个问题要考虑，一个是要不要买，另一个是买哪个旅行。

&emsp;&emsp;先看第一个问题，当前买不买是要看当天是否需要旅行，如果不需要的话，就和前一天的状态一致即可，因为不需要旅行的话就不需要考虑这一天的购入。

&emsp;&emsp;第二个问题会比较麻烦，因为ticket有三种，1天、7天和30天。所以对于某一天需要旅行的话，可以有三个选择：

- 前一天的cost加上1天的ticket；
- 7天前的cost加上7天的ticket；
- 30天前的cost加上30天的ticket

------

## Solution

&emsp;&emsp;上面的分析就是整个状态转移的过程了。当然在coding过程中还是有一些要注意的。首先是如果知道某一天是否需要旅游，我们可以在初始化dp数组的时候，把不需要旅游的dp初始化为`MAX_INT`，这样就很容易解决第一个问题。

&emsp;&emsp;在解决第二个问题的时候，会有边界的情况出现，这个时候需要防止越界的情况出现。比如在第10天的时候计算30天前，就应该用第一天的cost来计算。

------

## Code

```c++
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        vector<int> dp(366, INT_MAX);
        int size = days.size();
        
        for (int i = 0; i < size; i++) {
            dp[days[i]] = 0;
        }
        
        dp[0] = 0;
        
        for (int i = 1; i < 366; i++) {
            if (dp[i] == INT_MAX) {
                dp[i] = dp[i - 1];
            } else {
                int current = dp[i - 1] + costs[0];
                current = min(current, dp[max(0, i - 7)] + costs[1]);
                current = min(current, dp[max(0, i - 30)] + costs[2]);
                dp[i] = current;
            }
        }
        
        return dp[days[size - 1]];
    }
};
```

------

## Summary

&emsp;&emsp;这道题的本质还是最基本的动态规划，不过在这个基础上加上了多个选择，还有一些边界情况需要处理，总的来说难度不大。这道题的分析到这里，谢谢！
