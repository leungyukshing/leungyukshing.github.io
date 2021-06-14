---
title: LeetCode解题报告（398) -- 1690. Stone Game VII
  - LeetCode
mathjax: true
date: 2021-06-14 18:40:24

---

## Problem

Alice and Bob take turns playing a game, with **Alice starting first**.

There are `n` stones arranged in a row. On each player's turn, they can **remove** either the leftmost stone or the rightmost stone from the row and receive points equal to the **sum** of the remaining stones' values in the row. The winner is the one with the higher score when there are no stones left to remove.

Bob found that he will always lose this game (poor Bob, he always loses), so he decided to **minimize the score's difference**. Alice's goal is to **maximize the difference** in the score.

Given an array of integers `stones` where `stones[i]` represents the value of the `ith` stone **from the left**, return *the **difference** in Alice and Bob's score if they both play **optimally**.*

<!-- more -->

**Example 1:**

```
Input: stones = [5,3,1,4,2]
Output: 6
Explanation: 
- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
The score difference is 18 - 12 = 6.
```

**Example 2:**

```
Input: stones = [7,90,5,1,100,10,10,2]
Output: 122
```



**Constraints:**

- `n == stones.length`
- `2 <= n <= 1000`
- `1 <= stones[i] <= 1000`

------

## Analysis

&emsp;&emsp;这是一道博弈题目，博弈的原则就是双方按照自己的最优解去行动。对于一方来说，它的最优解有两部分，**一是自己的最优解，另一个是别人的最差解**。再来看题目，给出一个数组，双方从数组中的两头取元素，每次的得分是取走之后剩下元素的和，要求最后的得分最高。定义`dp[i][j]`为数组剩余`A[i:j]`时先手的最高得分，那么答案就是`dp[0][size - 1]`。然后我们来看转移方程：

- 如果选择第`i`个元素，则得分是`A[i+1: j]`，此时`dp[i][j] = sum(A[i+1: j]) - dp[i+1][j]`；
- 如果选择第`j`个元素，则得分是`A[i: j-1]`，此时`dp[i][j] = sum(A[i: j-1]) - dp[i][j-1]`。

&emsp;&emsp;转移时取上面两种情况较大的即可，问题就变成了怎么求`sum(A[i+1: j])`和`sum(A[i: j-1])`。求区间的和很常用的一种方法就是前缀和，我们计算`presum`，从1开始计算，然后有：

- `sum(A[i+1: j]) = presum[j + 1] - presum[i + 1]`；
- `sum(A[i: j-1]) = presum[j] - presum[i]`

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int stoneGameVII(vector<int>& stones) {
        int size = stones.size();
        vector<vector<int>> dp(size, vector<int>(size, 0));
        vector<int> presum(size + 1, 0);
        for (int i = 0; i < size; i++) {
            presum[i + 1] = presum[i] + stones[i];
        }
        
        for (int len = 2; len <= size; len++) {
            for (int i = 0; i + len - 1 < size; i++) {
                int j = i + len - 1;
                dp[i][j] = max(presum[j + 1] - presum[i + 1] - dp[i + 1][j], presum[j] - presum[i] - dp[i][j - 1]);
            }
        }
        return dp[0][size - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道博弈类型的dp，基本的结构已经介绍了。同时这道题目另一个值得总结的点是使用presum计算区间和。这道题目的分享到这里，感谢你的支持！
