---
title: LeetCode解题报告（418) -- 256. Paint House
mathjax: true
date: 2021-12-12 17:50:11
---

## Problem

There is a row of `n` houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an `n x 3` cost matrix `costs`.

- For example, `costs[0][0]` is the cost of painting house `0` with the color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on...

Return *the minimum cost to paint all houses*.

<!-- more -->

**Example 1:**

```
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
```

**Example 2:**

```
Input: costs = [[7,6,2]]
Output: 2
```

**Constraints:**

- `costs.length == n`
- `costs[i].length == 3`
- `1 <= n <= 100`
- `1 <= costs[i][j] <= 20`

------

## Analysis

&emsp;&emsp;这是一个刷房子问题，说是有三种颜色red, blue, green。每个房子刷不同颜色都有不同的cost，同时两个相邻的房子不能刷同一种颜色。首先这个题意很明确是指向dp，原因是，当前这个房子刷什么颜色，取决于上个房子刷了什么颜色（因为上一个房子刷了red，当前这个房子就不能刷red）。同时还带有极小值的问题，所以我们直接就往dp上靠了。

&emsp;&emsp;说到dp还是按着套路走，先找状态，再找转移方程。首先房子肯定是一个状态，其次颜色也是一个状态。确定了两个状态之后，dp数组就是二维的，然后遍历每一个状态。

&emsp;&emsp;接下来就看转移方程，对于`dp[i][j]`来说，能刷什么颜色完全取决于`dp[i - 1]`，也就是说，如果`dp[i - 1]`刷了red，当前这个就只能是blue和green中取一个较小的。所以这里有一个双层的循环。遍历所有的颜色`k`，如果`k == j`则忽略，如果`k != j`才取一个较小的。这里只有三种颜色，也可以手动列举，但我建议还是写一个循环，因为这样能够清晰地知道状态转移，还容易扩展到颜色有`n`种的情况。

&emsp;&emsp;最后我们只需要找出`dp[i - 1]`中三种颜色取值最小的那个就可以了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        int size = costs.size();
        vector<vector<int>> dp(size, vector<int>(3, INT_MAX));
        
        dp[0][0] = costs[0][0];
        dp[0][1] = costs[0][1];
        dp[0][2] = costs[0][2];
        
        for (int i = 1; i < size; ++i) {
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 3; ++k) {
                    if (k == j) {
                        continue;
                    }
                    dp[i][j] = min(dp[i - 1][k] + costs[i][j], dp[i][j]);
                }
            }
        }
        return min({dp[size - 1][0], dp[size - 1][1], dp[size - 1][2]});
    }
};
```

------

## Summary

&emsp;&emsp;这道题目题意很简单，对dp的指向很明显，转移逻辑也很容易，我认为是比较容易的dp题目，当然如果考虑到拓展性，代码可以写的更加general一些，这道题目是一组系列题，后面还会聊到这部分。这道题目的分享到这里，感谢你的支持！
