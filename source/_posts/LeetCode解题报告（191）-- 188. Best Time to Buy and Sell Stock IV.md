---
title: LeetCode解题报告（191）-- 188. Best Time to Buy and Sell Stock IV
tags:
  - LeetCode
mathjax: true
date: 2020-10-18 22:46:32

---

## Problem

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Design an algorithm to find the maximum profit. You may complete at most `k` transactions.

**Notice** that you may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

<!-- more -->

**Example 1:**

```
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

**Example 2:**

```
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

**Constraints:**

- `0 <= k <= 109`
- `0 <= prices.length <= 1i4`
- `0 <= prices[i] <= 1000`

------

## Analysis

&emsp;&emsp;这道题目是股票买卖系列题的第四题，题目的细节要求这里就不再叙述了，就是买入和卖出。这道题目不一样的地方在于，它对交易次数有限制，一买一卖算是一次交易。这里有两个限制条件，一个是天数，第二个是交易的次数。同类型的题目，不过加多了一个限制条件，dp由一维转为二维。定义两个dp状态，

- `local[i][j]`：在到达第`i`天时最多可进行`j`次交易，并且最后一催交易在最后一天卖出的最大利润。
- `global[i][j]`：在到达第`i`天时最多可进行`j`次交易的最大利润。

&emsp;&emsp;状态转移方程为：

- `local[i][j] = max(global[i - 1][j - 1] + max(diff, 0), local[i - 1][j] + diff)`
- `global[i][j] = max(local[i][j], global[i - 1][j])`

&emsp;&emsp;这里有一个坑需要注意，如果当最大交易次数`k`远大于prices的天数时，这道题目就退化成之前的题目了。因此就不需要用dp来求解了。

------

## Solution

&emsp;&emsp;两个状态转移都是从上到下，从左到右的，所以用一个一维数组就可以了。在处理`k`远大于天数的情况，复用前面系列题的逻辑即可。

------

## Code

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        if (prices.empty()) {
            return 0;
        }
        if (k >= prices.size()) {
            return solveMaxProfit(prices);
        }
        
        vector<int> global(k + 1, 0);
        vector<int> local(k + 1, 0);
        
        
        for (int i = 0; i < prices.size() - 1; i++) {
            int diff = prices[i + 1] - prices[i];
            for (int j = k; j >= 1; j--) {
                local[j] = max(global[j - 1] + max(diff, 0), local[j] + diff);
                global[j] = max(global[j], local[j]);
            }
        }
        return global[k];
    }
private:
    int solveMaxProfit(vector<int>& prices) {
        int result = 0;
        for (int i = 1; i < prices.size(); i++) {
            if (prices[i] - prices[i - 1] > 0) {
                result += prices[i] - prices[i - 1];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;有了前面几题的铺垫，再做这一题难度会降低不少。难点在于划分出局部最优和全局最优两个状态转移方程，同时还要考虑到一些特殊的case。这道题目的分享到这里，谢谢！
