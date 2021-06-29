---
title: LeetCode解题报告（319)-- 714. Best Time to Buy and Sell Stock with Transaction Fee
tags:
  - LeetCode
mathjax: true
abbrlink: 45593
date: 2021-03-20 11:44:07
---

## Problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

<!-- more -->

**Example 1:**

```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**Example 2:**

```
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
```

**Constraints:**

- `1 <= prices.length <= 5 * 104`
- `1 <= prices[i] < 5 * 104`
- `0 <= fee < 5 * 104`

------

## Analysis

&emsp;&emsp;股票买卖类的系列题，解题的大方向还是动态规划，因为买卖都有，所以还是使用两个状态：

- `hold[i]`：第`i`天持有股票的最大收益；
- `sold[i]`：第`i`天没有持有股票的最大收益。

&emsp;&emsp; 然后我们来看看状态转移方程怎么写。收益的变化只有在买卖的时候才会有，所以对于某一天来说，如果什么都不做的话，应该是和前一天的收益是一样的。然后我们要考虑买卖发生的情况，因为股票不能重复购买或重复抛售，只能一买一卖，所以交易发生只有两种情况：

1. 第`i - 1`天是持有状态，第`i`天卖出；
2. 第`i - 1`天是抛售状态，第`i`天买入。

&emsp;&emsp;我们根据上面两种状态来写状态转移方程即可，买入的时候减去当天股票的价格即可，卖出的时候需要加上当前股票的价格，但是还要减去手续费。然后结果是最后一天抛售的结果。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int size = prices.size();
        vector<int> sold(size, 0), hold(size, 0);
        
        hold[0] = -prices[0];
        for (int i = 1; i < size; i++) {
            hold[i] = max(hold[i - 1], sold[i - 1] - prices[i]);
            sold[i] = max(sold[i - 1], hold[i - 1] + prices[i] - fee);
        }
        return sold[size - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道题是比较简单的dp，条件和状态转移都不难，主要是理清买卖的关系和发生条件。这道题目的分享到这里，感谢你的支持！
