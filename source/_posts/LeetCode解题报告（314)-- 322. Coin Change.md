---
title: LeetCode解题报告（314)-- 322. Coin Change
tags:
  - LeetCode
mathjax: true
date: 2021-03-12 21:14:26

---

## Problem

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

<!-- more -->

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

**Example 4:**

```
Input: coins = [1], amount = 1
Output: 1
```

**Example 5:**

```
Input: coins = [1], amount = 2
Output: 2
```



**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

------

## Analysis

&emsp;&emsp;题目给出一些硬币的面值，以及总金额，要求用最少的硬币数量去凑出这个总金额。因为每个硬币可以使用也可以不使用，可以使用一次，也可以使用多次，所以这实际上是一个很典型的dp题目。我们定义`dp[i]`为凑成金额`i`所需要的最少的硬币数量，这样从金额少开始计算，金额大的就能够从前面的状态计算出来。

&emsp;&emsp;然后我们再来看状态转移方程，对`coins[j]`，我们有两种选择，第一个是选择这个硬币，此时就需要`dp[i - coins[j]]`的数量加上这一个硬币，第二个是不选择这个硬币，此时数量就是`dp[i]`了，两者取最小值即可。

------

## Solution

&emsp;&emsp;注意处理边界情况，如果硬币不能组成总额，需要怎么判断呢？因为硬币最小是1块，所以对于总金额`amount`来说，最多就是用`amount`个硬币，因此在初始化dp数组时，就初始化为`amount + 1`，如果最后的结果是`amount + 1`，说明没有组合能够组合出`amount`，返回-1即可。

------

## Code

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 0; i <= amount; i++) {
            for (int j = 0; j < coins.size(); j++) {
                if (coins[j] <= i) {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return (dp[amount] > amount) ? -1: dp[amount];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的dp题目，难点在于两个循环进行。金额要从小到大，然后逐个硬币进行计算。这道题目的分享到这里，感谢你的支持！
