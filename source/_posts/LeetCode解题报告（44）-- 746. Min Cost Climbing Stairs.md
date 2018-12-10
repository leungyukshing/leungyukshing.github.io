---
title: LeetCode解题报告（44）-- 746. Min Cost Climbing Stairs
tags:
  - LeetCode
mathjax: true
abbrlink: 39530
date: 2018-12-10 12:24:34
---
## Problem
On a staircase, the `i`-th step has some non-negative cost `cost[i]` assigned (0 indexed).

Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.
<!-- more -->

**Example 1:**
```
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
```

**Example 2:**
```
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].
```

**Note:**
  1. `cost` will have a length in the range `[2, 1000]`.
  2. Every `cost[i]` will be an integer in the range `[0, 999]`.

## Analysis
&emsp;&emsp;这是一题关于动态规划的题目，难度不大但很有代表性。题目很简单，给出每个楼梯的cost，我们需要求出走到楼顶的最小代价。先不考虑特殊情况，对于某一层`i`来说，要么我从`i-1`层上来，要么从`i-2`层上来。所以我们只需要最小化这个转移过程就可以了。
&emsp;&emsp;另外还有几个特别的状态。第一，当我们位于第0层或者第1层，代价是0。因为题目说了我们可以从第一层开始或者第二层开始。另一个问题就是，题目给出的`cost`数组应该是每一步（每一级楼梯）的代价，因此如果有n个代价的话，顶楼应该是n+1楼。例如，有两个代价的话，应该有三层楼，因此我们的代价转移函数`f`大小应该有`n+1`个元素，其中，我们需要的是最后一个元素，即`f[n]`。
&emsp;&emsp;状态转移方程：
$$
f[i] = \begin{cases}
        0,  & \text{if $i=0$ or $i=1$} \\
        min(f[i-1]+cost[i-1], f[i-2]+cost[i-2]), & \text{if $i \geq2$}
        \end{cases}
$$

## Solution
&emsp;&emsp;使用一个vector`f`来存储状态转移数据。按照状态转移方程从左到右求出到达每一层楼的最小代价，最后返回`f[n]`即可。

## Code
```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int size = cost.size() + 1;
        vector<int> f(size, 0);

        for (int i = 0; i < size; i++) {
            if (i == 0) {
                f[0] = 0;
            }
            else if (i == 1) {
                f[1] = 0;
            }
            else {
                f[i] = min(f[i-1] + cost[i-1], f[i - 2] + cost[i-2]);
            }
        }

        return f[size-1];
    }
};
```
**运行时间：**约4ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;这道题本身难度不大，但是是一道很典型的线性规划的题目。我认为解这一类题目的技巧是先写出状态转移方程，然后对着这个方程coding就可以了。这一类题目有时候理解起来会有点抽象，可以先在纸上演算几个例子。本题的分析就到这里了，谢谢您的支持！
