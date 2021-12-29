---
title: LeetCode解题报告（474）-- 1705. Maximum Number of Eaten Apples
mathjax: true
date: 2021-12-29 16:23:51
---

## Problem

There is a special kind of apple tree that grows apples every day for `n` days. On the `ith` day, the tree grows `apples[i]` apples that will rot after `days[i]` days, that is on day `i + days[i]` the apples will be rotten and cannot be eaten. On some days, the apple tree does not grow any apples, which are denoted by `apples[i] == 0` and `days[i] == 0`.

You decided to eat **at most** one apple a day (to keep the doctors away). Note that you can keep eating after the first `n` days.

Given two integer arrays `days` and `apples` of length `n`, return *the maximum number of apples you can eat.*

<!-- more -->

**Example 1:**

```
Input: apples = [1,2,3,5,2], days = [3,2,1,4,2]
Output: 7
Explanation: You can eat 7 apples:
- On the first day, you eat an apple that grew on the first day.
- On the second day, you eat an apple that grew on the second day.
- On the third day, you eat an apple that grew on the second day. After this day, the apples that grew on the third day rot.
- On the fourth to the seventh days, you eat apples that grew on the fourth day.
```

**Example 2:**

```
Input: apples = [3,0,0,0,0,2], days = [3,0,0,0,0,2]
Output: 5
Explanation: You can eat 5 apples:
- On the first to the third day you eat apples that grew on the first day.
- Do nothing on the fouth and fifth days.
- On the sixth and seventh days you eat apples that grew on the sixth day.
```



**Constraints:**

- `n == apples.length == days.length`
- `1 <= n <= 2 * 104`
- `0 <= apples[i], days[i] <= 2 * 104`
- `days[i] = 0` if and only if `apples[i] = 0`.

---

## Analysis

&emsp;&emsp;题目给出两个数组`apples`和`days`，`apples[i]`代表第`i`天生产了多少个苹果，`days[i]`代表第`i`天生产的苹果几天后腐烂，所以能够使用的天数就是`i + days[i] - 1`。这道题目其实很贴近实际，食品都有一个保质期，那为了能吃到最多的苹果，很直接的做法就是每次都吃快要过期的，这种吃法肯定是能吃最多的。

&emsp;&emsp;所以这就是一个贪心算法，而贪心算法一般结合priority queue做，这里就是按照过期时间来排序。遍历数组，先把当前的苹果加进去，然后把已经过期的pop掉，如果还有剩，就吃一个，数量减1。

## Solution

&emsp;&emsp;需要注意外层大循环的条件，不只是`i < n`，还有`!pq.empty()`，因为题目说了不生产之后，还是可以继续吃的，知道没得吃或者都烂掉了。

------

## Code

```c++
struct cmp {
    bool operator()(const pair<int, int> &a, const pair<int, int> &b) {
        return a.second > b.second;
    }
};

class Solution {
public:
    int eatenApples(vector<int>& apples, vector<int>& days) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> pq;
        
        int i = 0; // current day
        
        int result = 0;
        int n = apples.size();
        while (i < n || !pq.empty()) {
            if (pq.empty() || i < n) {
                pq.push({apples[i], i + days[i] - 1});
            }
            
            while (!pq.empty() && pq.top().second < i) {
                pq.pop();
            }
            
            if (!pq.empty()) {
                auto cur = pq.top();
                pq.pop();
                ++result;
                if (cur.first - 1 > 0) {
                    pq.push({cur.first - 1, cur.second});
                }
            }
            ++i;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是非常经典的使用priority queue解决贪心问题，难度不大。这道题目的分享到这里，感谢你的支持！

