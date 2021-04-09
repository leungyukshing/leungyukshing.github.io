---
title: LeetCode解题报告（334) -- 474. Ones and Zeroes
tags:
  - LeetCode
mathjax: true
date: 2021-04-06 13:24:17

---

## Problem

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return *the size of the largest subset of strs such that there are **at most*** `m` `0`*'s and* `n` `1`*'s in the subset*.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

<!-- more -->

**Example 1:**

```
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
```

**Example 2:**

```
Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.
```

**Constraints:**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` consists only of digits `'0'` and `'1'`.
- `1 <= m, n <= 100`

------

## Analysis

&emsp;&emsp;题目给出了一个字符串数组，然后分别给出了0和1的数量限制，问最多能够拿多少个字符串出来，使得他们组成的子集中，0和1出现的次数不超过限制。有了两个限制条件，分别是0和1的数量，又是求子集的大小，看到这种形式就可以往dp上想了。

&emsp;&emsp;因为有两个限制条件，所以dp的数组是二维的，一个维度表示0的数量，另一个维度表示1的数量，据此我们可以定义`dp[i][j]`为，当0的数量最多为i，1的数量最多为j时，符合要求的最大的子集的大小，所以答案就是`dp[m][n]`了。

&emsp;&emsp;接下来分析状态转移方程。假设0和1的数量都是没有限制的，先来看当`dp[1][1] = 2`时，说明当前0的数量最多为1，1的数量最多为1的时候，这个子集的大小是，此时来了一个新的字符串`110`，那么`dp[2][3]`就应该更新为3。我是怎么找到这个2、3呢？是因为在`110`中，0的数量有1个，1的数量有2个，所以`i`的遍历范围应该是在原来的基础上+1，而`j`的遍历范围应该是在原来的基础上+2。那么，他们的上限是什么呢？因为前面讨论的时候先把限制去掉了，但是实际题目中的限制分别是`m`和`n`，所以遍历的范围也就一目了然了：

- 对`i`而言，范围是`[m - zeros, m]`，这是因为字符串里面有了zeros个0，所以只要就是从`m - zeros`上开始计算，如果再比这个小的话，就凑不够数了；
- 同理，对`j`而言，范围是`[n - ones, n ]`;

&emsp;&emsp;确认了遍历的范围后，就可以看状态转移的方向了，从上面可以知道，我们能够从0和1较小的状态计算出较大的状态，但是这里并不意味着二维数组的遍历就要从左上角开始。原因是，对于同一个字符串而言，**每个dp状态的计算是基于上一个字符串计算的结果得到的，而不是当前字符串下的上一个dp状态**。所以我们采取的方向是从松到紧，先从最宽松的限制状态开始，向严格的状态遍历。即：`dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1 )`。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        int size = strs.size();
        int dp[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                dp[i][j] = 0;
            }
        }
        
        for (string str : strs) {
            int zeros = 0, ones = 0;
            for (char c : str) (c == '0') ? ++zeros : ++ones;
            for (int i = m; i >= zeros; --i) {
                for (int j = n; j >= ones; --j) {
                    dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

------

## Summary

&emsp;&emsp;这是一道比较难的DP题目，虽然很快能够分析出使用dp，但是对于dp的状态转移及遍历的方向，还是花了很多的时间去思考，一次没搞懂没关系，后面再遇到类似的时候再找找规律。这道题目的分享到这里，感谢你的支持！
