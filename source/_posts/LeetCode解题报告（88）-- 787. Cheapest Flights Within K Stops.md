---
title: LeetCode解题报告（88）-- 787. Cheapest Flights Within K Stops
tags:
  - LeetCode
date: 2020-06-15 11:49:30
mathjax: true

---

## Problem

There are `n` cities connected by `m` flights. Each flight starts from city `u` and arrives at `v` with a price `w`.

Now given all the cities and flights, together with starting city `src` and the destination `dst`, your task is to find the cheapest price from `src` to `dst` with up to `k` stops. If there is no such route, output `-1`.

<!-- more -->

**Example 1:**

```
Example 1:
Input: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
Output: 200

The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.
```

The graph looks like this:

![Example1](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

**Example 2:**

```
Example 2:
Input: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
Output: 500
Explanation: 
The graph looks like this:


The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.
```

The graph looks like this:

![Example2](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

**Constraints:**

- The number of nodes `n` will be in range `[1, 100]`, with nodes labeled from `0` to `n`` - 1`.
- The size of `flights` will be in range `[0, n * (n - 1) / 2]`.
- The format of each flight will be `(src, ``dst``, price)`.
- The price of each flight will be in the range `[1, 10000]`.
- `k` is in the range of `[0, n - 1]`.
- There will not be any duplicated flights or self cycles.

------

## Analysis

&emsp;&emsp;题目要求计算的是最短路径，限制条件是stop的次数。注意stop的次数和飞行的次数并不是相等的，为了和计算价钱同步，这里我用的是飞行的次数，也就是`stop + 1`次。对于每一个city，记录从`src`到该city的最小的cost。状态转移方程比较简单，使用上一次的city加上当次飞行的cost就是到该city的cost了。

------

## Solution

&emsp;&emsp;使用二维数组记录dp状态，初始化的时候除了`src`其余city都是不可达的，所以cost设置为一个很大的数字就可以了。

------

## Code

```c++
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<vector<int>> dp(K + 2, vector<int>(n, 1e9));
        
        dp[0][src] = 0;
        for (int i = 1; i < K + 2; i++) {
            dp[i][src] = 0;
            for (int j = 0; j < flights.size(); j++) {
                dp[i][flights[j][1]] = min(dp[i][flights[j][1]], dp[i - 1][flights[j][0]] + flights[j][2]);
            }
        }
        
        return dp[K + 1][dst] == 1e9? -1: dp[K + 1][dst];
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目主要的考点还是DP，比较难的是要想到从stop的次数开始遍历，后面状态转移方程相对来说比较简单。这道题目的分享到此为止，感谢您的支持，欢迎转发、评论，分享，谢谢！
