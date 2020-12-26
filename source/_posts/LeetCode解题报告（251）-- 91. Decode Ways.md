---
title: LeetCode解题报告（251）-- 91. Decode Ways
tags:
  - LeetCode
mathjax: true
date: 2020-12-27 00:36:11


---

## Problem

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a **non-empty** string containing only digits, determine the total number of ways to decode it.

The answer is guaranteed to fit in a **32-bit** integer.

<!-- more -->

**Example 1:**

```
Input: s = "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: s = "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**

```
Input: s = "0"
Output: 0
Explanation: There is no character that is mapped to a number starting with '0'. We cannot ignore a zero when we face it while decoding. So, each '0' should be part of "10" --> 'J' or "20" --> 'T'.
```

**Example 4:**

```
Input: s = "1"
Output: 1
```

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).

------

## Analysis

&emsp;&emsp;这道题的背景是字符串和数字的解码，但是考察的并不是字符串的处理，而是一个组合数量的问题。从Example 1就能看到，这道题目的难点在于数字的组合情况，即两个数字的情况下，有可能是有两种不同的decode形式。因为是字符串的形式，所以往后一直都会有这种重叠的逻辑。同时可以看到，在1的时候，只有一种decode；在12的时候，实际上是包含了第一种decode（1、2），再新加了一种decode（12）。所以这里看出来，后面的状态是在前面的基础上计算得出的。啊，原来这是一道dp！

&emsp;&emsp;确定使用dp求解后，我们就来看状态：定义**dp[i]为字符串中前i个字符解码组合数量**，并初始化`dp[0] = 1`。首先，对于某个`i`来说，它肯定包含前面`i-1`的组合数量，我们要判断的是它能否和前面的数组合成一个新的小于26并大于10的数字，如果满足的话，需要把`dp[i - 2]`也要加上，相当于把这个两位数看作是一个数。所以状态转移方程为：

1. 满足两位数的时候：`dp[i] = dp[i - 1] + dp[i - 2]`；
2. 不满足两位数的时候：`dp[i] = dp[i - 1]`

------

## Solution

&emsp;&emsp;根据上面的状态转移方程coding即可，还需要注意一些边界的case。

1. 如果开头为0的话可以直接return，因为不可能有开头为0的encoded string；
2. 中间的0对应的dp值为0，因为0本身不可能有decode对应的字母，它必须和前面的进行组合；
3. 第一个字符不用考虑两位数的情况。

------

## Code

```c++
class Solution {
public:
    int numDecodings(string s) {
        if (s.empty() || s[0] == '0') {
            return 0;
        }
        vector<int> dp(s.size() + 1, 0);
        
        dp[0] = 1;
        
        for (int i = 1; i < dp.size(); i++) {
            dp[i] = s[i - 1] == '0' ? 0: dp[i - 1];
            if (i > 1 && (s[i - 2] == '1' || s[i - 2] == '2' && s[i - 1] <= '6')) {
                // search previous digit
                dp[i] += dp[i - 2];
            }
        }
        return dp.back();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道比较有难度的dp题目。在字符串中做dp是比较少见的，这里也稍作总结。这道题目的分享到这里，谢谢您的支持！
