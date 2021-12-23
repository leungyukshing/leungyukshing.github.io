---
title: LeetCode解题报告（439）-- 727. Minimum Window Subsequence
mathjax: true
date: 2021-12-22 23:28:31
---

## Problem

Given strings `s1` and `s2`, return *the minimum contiguous substring part of* `s1`*, so that* `s2` *is a subsequence of the part*.

If there is no such window in `s1` that covers all characters in `s2`, return the empty string `""`. If there are multiple such minimum-length windows, return the one with the **left-most starting index**.

<!-- more -->

**Example 1:**

```
Input: s1 = "abcdebdde", s2 = "bde"
Output: "bcde"
Explanation: 
"bcde" is the answer because it occurs before "bdde" which has the same length.
"deb" is not a smaller window because the elements of s2 in the window must occur in order.
```

**Example 2:**

```
Input: s1 = "jmeqksfrsdcmsiwvaovztaqenprpvnbstl", s2 = "u"
Output: ""
```

**Constraints:**

- `1 <= s1.length <= 2 * 104`
- `1 <= s2.length <= 100`
- `s1` and `s2` consist of lowercase English letters.

------

## Analysis

&emsp;&emsp;题目给出了两个字符串`s1`和`s2`，然后要求在`s1`中找到一个substring（连续的），使得`s2`是这个substring的subsequence。首先这种两个字符串的题目，又涉及到substring、subsequence，一般就可以往dp方面想了。通常来说是一个二维的dp，那么这里的方程怎么确定呢？还是按照老套路，`dp[i][j]`表示`s1`中前`i`个字符组成的substring能使得`s2`的前`j`个字符是sequence的**初始位置**。例如，`s1 = dbd`，`s2 = bd`，那么`dp[2][1] = 1`，因为在`s1`中起始下标是1。

&emsp;&emsp;然后我们来看转移方程，如果`s1[i] == s2[j]`就很好办，因为不会影响起始位置，所以`dp[i][j] = dp[i - 1][j - 1]`；如果`s1[i] != s2[j]`，那么只能是`s1`向前找了，所以是`dp[i - 1][j]`。

&emsp;&emsp;最后，因为答案要求的是返回`s1`中的substring，所以要记录初始位置以及最小的长度。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    string minWindow(string s1, string s2) {
        int m = s1.size(), n = s2.size(), start = -1, minLen = INT_MAX;
        
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, -1));
        
        for (int i = 0; i <= m; ++i) {
            dp[i][0] = i;
        }
        
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= min(i, n); ++j) {
                dp[i][j] = (s1[i - 1] == s2[j - 1] ? dp[i - 1][j - 1] : dp[i - 1][j]);
            }
            if (dp[i][n] != -1) {
                int len = i - dp[i][n];
                if (len < minLen) {
                    minLen = len;
                    start = dp[i][n];
                }
            }
        }
        return (start == -1) ? "": s1.substr(start, minLen);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是两个字符串dp的变种题目，难度在于要记录的是起始位置，同时状态转移也需要有一些思考。这道题目的分享到这里，感谢你的支持！
