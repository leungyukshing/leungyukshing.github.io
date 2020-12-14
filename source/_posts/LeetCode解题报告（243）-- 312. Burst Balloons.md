---
title: LeetCode解题报告（243）-- 312. Burst Balloons
tags:
  - LeetCode
mathjax: true
date: 2020-12-14 11:48:20

---

## Problem

Given `n` balloons, indexed from `0` to `n-1`. Each balloon is painted with a number on it represented by array `nums`. You are asked to burst all the balloons. If the you burst balloon `i` you will get `nums[left] * nums[i] * nums[right]` coins. Here `left` and `right` are adjacent indices of `i`. After the burst, the `left` and `right` then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

- You may imagine `nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them.
- 0 ≤ `n` ≤ 500, 0 ≤ `nums[i]` ≤ 100

<!-- more -->

**Example:**

```
Input: [3,1,5,8]
Output: 167 
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

------

## Analysis

&emsp;&emsp;题目给定一个数组，每次从数组中移除一个数字`nums[i]`，移除时的分数是`nums[i - 1] * nums[i] * nums[i + 1]`，问能够获得的最多的分数是多少？这类题目是极值问题，而且当前状态是基于上一个状态的，所以解题的思路自然就是dp。定义状态方程`dp[i][j]`为**区间[i, j]的最高得分**。假设我们要求`dp[1][3]`，有如下几种情况：

1. 如果先打破气球1，而且之前也已经计算好`dp[2][3]`的话，就可以直接更新`dp[1][3]`了；
2. 如果先打破气球2，而且之前已经计算好`dp[1][1]`和`dp[3][3]`，就可以直接更新`dp[1][3]`；
3. 如果先打破气球3，而且之前已经计算好`dp[1][2]`，就可以直接更新`dp[1][3]`/

&emsp;&emsp;从上面三个元素的区间，可以理解到状态转移，但是对于包含超过3个元素的区间呢？实际上大区间的值是通过小区间构成的，假设要求`dp[i][j]`，因为要计算小区间，假设第`k`个气球最后被打爆（$k \in [i, j]$），那么此时区间`[i, j]`被分为了三个部分：`[i, k - 1]`、`[k]`、`[k + 1, j]`。因为第`k`个气球是最后打爆，所以`[i, k - 1]`和`[k + 1, j]`这两个区间的值之前已经计算出来，所以这两个圈的得分是可以直接加进去的。但是要注意到，现在区间`[i, j]`里面只剩下`k`这个气球，所以他左右相邻的气球并不是`k - 1` 和`k + 1`，而应该是`i - 1`和`j + 1`。到这里之后，状态转移方程就得出来了：
$$
dp[i][j] = max(dp[i][j], nums[i - 1] \times nums[k] \times nums[j + 1] + dp[i][k - 1] + dp[k + 1][j])
$$
&emsp;&emsp;确定好状态转移方程后，就要来看遍历的顺序。因为大区间的值是有其子区间计算得到，所以遍历的顺序应该是先遍历子区间，再遍历父区间。

------

## Solution

&emsp;&emsp;在计算区间问题时，在算到头尾的时候会遇到越界的问题，为了减少代码的判断逻辑，可以直接在头尾插入1，只有我们实际上计算的就是`dp[1][n]`。

------

## Code

```c++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int size = nums.size();
        nums.insert(nums.begin(), 1); // preceding one
        nums.push_back(1); // tailing one
        vector<vector<int>> dp(size + 2, vector<int>(size + 2, 0)); // dp
        
        for (int len = 1; len <= size; len++) {
            for (int i = 1; i <= size - len + 1; i++) {
                int j = i + len - 1;
                for (int k = i; k <= j; k++) {
                    dp[i][j] = max(dp[i][j], nums[i - 1] * nums[k] * nums[j + 1] + dp[i][k - 1] + dp[k + 1][j]);
                }
            }
        }
        return dp[1][size];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较复杂的dp，非常值得思考和总结。像这种比较复杂的dp问题，凭借经验比较难找到转移方程，所以要通过例子去分析。先考虑3个的情况，再考虑多个的情况。同时还需要注意遍历的顺序。这道题目的分享到这里，谢谢您的支持！
