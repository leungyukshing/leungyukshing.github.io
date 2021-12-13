---
title: LeetCode解题报告（419） -- 265. Paint House II
mathjax: true
date: 2021-12-12 18:04:52
---

## Problem

There are a row of `n` houses, each house can be painted with one of the `k` colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an `n x k` cost matrix costs.

- For example, `costs[0][0]` is the cost of painting house `0` with color `0`; `costs[1][2]` is the cost of painting house `1` with color `2`, and so on...

Return *the minimum cost to paint all houses*.

<!-- more -->

**Example 1:**

```
Input: costs = [[1,5,3],[2,9,4]]
Output: 5
Explanation:
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.
```

**Example 2:**

```
Input: costs = [[1,3],[2,4]]
Output: 5
```

**Constraints:**

- `costs.length == n`
- `costs[i].length == k`
- `1 <= n <= 100`
- `2 <= k <= 20`
- `1 <= costs[i][j] <= 20`

 

**Follow up:** Could you solve it in `O(nk)` runtime?

------

## Analysis

&emsp;&emsp;这是刷房子系列的第二题，如果没有做过第一题的先看一下我的解题博客[Paint House](https://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88418%EF%BC%89--%20256.%20Paint%20House.html)，在里面我的解法是能支持多种颜色的，所以换句话说，只需要把颜色的种类从3换成一个变量，就可以直接解出这道题目。但是，这道题目的follow up提出在$O(nk)$的时间复杂度内解决，而我之前的解法是$O(nk^2)$的。

&emsp;&emsp;这是不是说之前的方法不可行呢？当然不是。先来看看$O(k^2)$是怎么产生的。因为在处理`dp[i][j]`时，要根据`dp[i - 1][k] (k != j)`都找一遍取一个最小的，所以这里对颜色的种类`k`做一个双循环。优化的关键就要在这里下功夫了，这里其实我们只需要找一个最小的，那我们可以不用双重循环的方法，而是先遍历一遍`dp[i - 1]`找到最小的`k`，然后去更新即可。那么还有个问题就是当`j = k`的时候，怎么更新`dp[i][j]`，很简单，如果不能用最小的`k`去更新，那就找一个第二小的`k`。

&emsp;&emsp;所以归纳一下，优化的方法就是，先遍历`dp[i - 1]`的所有颜色状态，找出使得`dp[i][k]`最小的`k1`以及第二小的`k2`，最后再遍历一遍`dp[i - 1]`去更新，对于`j != k1`的情况，`dp[i][j]`就用`dp[i - 1][k1]`去更新；对于`j == k1`的情况，`dp[i][k1]`就用`dp[i - 1][k2]`去更新，这样复杂度就降下来了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        int size = costs.size(), numberOfColor = costs[0].size();
        
        vector<vector<int>> dp(size, vector<int>(numberOfColor, INT_MAX));
        // base case
        for (int j = 0; j < numberOfColor; ++j) {
            dp[0][j] = costs[0][j];
        }
        for (int i = 1; i < size; i++) {
            // find the minColor and the secondMinColor from previous row
            int minColor = -1, secondColor = -1;
            for (int j = 0; j < numberOfColor; j++) {
                if (minColor == -1 || dp[i - 1][j] < dp[i - 1][minColor]) {
                    secondColor = minColor;
                    minColor = j;
                } else if (secondColor == -1 || dp[i - 1][j] < dp[i - 1][secondColor]) {
                    secondColor = j;
                }
            }
            
            // update dp[i][j]
            for (int j = 0; j < numberOfColor; j++) {
                if (j == minColor) {
                    dp[i][j] = min(dp[i][j], dp[i - 1][secondColor] + costs[i][j]);
                } else {
                    dp[i][j] = min(dp[i][j], dp[i - 1][minColor] + costs[i][j]);
                }
            }
        }
        
        int result = INT_MAX;
        for (int j = 0; j < numberOfColor; ++j) {
            result = min(result, dp[size - 1][j]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目主要的思路还是基于上一道系列题，重点在于优化。这也给我们提供了一个优化dp复杂度的思路，就是在更新状态时，我们通常会直接用形如`dp[i] = min(dp[i - 1] + x)`的形式去更新，而实际上深入分析这个最小值无非就是两个数值，因此有时候是可以优化的。这道题目的分享到这里，感谢你的支持！
