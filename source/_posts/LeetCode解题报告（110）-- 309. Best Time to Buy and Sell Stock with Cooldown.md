---
title: LeetCode解题报告（110）-- 309. Best Time to Buy and Sell Stock with Cooldown
tags:
  - LeetCode
mathjax: true
abbrlink: 52086
date: 2020-07-31 12:09:33
---

## Problem

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

- You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
- After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

<!-- more -->

**Example:**

```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

------

## Analysis

&emsp;&emsp;这道题目是股票买卖问题，是非常经典的动态规划。这里有一个额外的要求就是卖掉股票后，需要cooldown一天才能买。我们想要知道什么时候买和卖是最好的，因为卖需要依赖于买入的信息，所以同时维护两个方程：

1. 第`i`天买入的最大收益

2. 第`i`天卖出的最大收益

   &emsp;&emsp;先看第一个，排除边界情况（第一次买入），买入一定是在卖出之后的，所以买入的最大收益是之前卖出的最大收益减去当天买入花的钱或者当天不操作。

$$
buy[i] = \max(sell[i - 2] - prices[i],buy[i - 1])
$$

&emsp;&emsp;再看第二个，卖出必须要在上一次的买入之后，所以卖出的最大收益是之前买入的最大收益加上当天卖出赚的钱或者当天不操作。
$$
sell[i] = \max(buy[i - 1] + prices[i], sell[i - 1])
$$

------

## Solution

&emsp;&emsp;动态转移方程按照上面的来写就可以，最终的结果就是最后一天卖出的最大收益，因为手上不需要再持有股票。需要注意的是，因为第一天必须要买入，所以要初始化`buy`。同时，在`buy`的动态转移方程中，考虑到cooldown，需要是大前天的`sell`，这里需要加一个边界判断。

------

## Code

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if (size == 0) {
            return 0;
        }
        vector<int> buy(size + 1, 0), sell(size + 1, 0);
        
        // must buy stock in first day
        buy[1] = -prices[0];
        
        for(int i = 2; i <= size; i++) {
            buy[i] = max(sell[i - 2] - prices[i - 1], buy[i - 1]);
            sell[i] = max(sell[i - 1], buy[i - 1] + prices[i - 1]);
        }
        return sell[size];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的买入卖出dp问题，但是这里有一个cooldown的变种，实际上动态转移方程还是一样的，只需要在买入的时候加上cooldown的考虑就可以了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
