---
title: LeetCode解题报告（428）-- 72. Edit Distance
mathjax: true
date: 2021-12-21 16:23:11
---

## Problem

Given two strings `word1` and `word2`, return *the minimum number of operations required to convert `word1` to `word2`*.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

<!-- more -->

**Example 1:**

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2:**

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

------

## Analysis

&emsp;&emsp;这是一道非常非常经典的动态规划题目，也是一个在工业实践中很常用的算法。首先我们来看什么是**编辑距离**。给定两个字符串`word1`和`word2`，通过一系列的操作把`word1`转化为`word2`，操作包括在`word1`中插入、删除、替换。这种两个字符串变换的题目本身就很适合二维dp。`dp[i][j]`表示把`word1`前`i`个字符转换成`word2`前`j`个字符所需要的最小编辑距离。

&emsp;&emsp;先来看base case。当`word2`长度为0时，要把`word1`变过去只能是删除所有的字符，所以`dp[i][0] = i`。同理，如果`word1`长度为0时，要把`word1`变成`word2`只能是添加字符，所以`dp[0][j] = j`。

&emsp;&emsp;然后来看转移方程。当`word1[i - 1] == word2[j - 1]`时，不需要编辑操作，所以`dp[i][j] = dp[i - 1][j - 1]`；当`word1[i - 1] != word2[j - 1]`时，有三种操作：

+ `word1[i - 1]`替换成`word2[j - 1]`，所以`dp[i][j] = dp[i - 1][j - 1]`；
+ 删除掉`word1[i - 1]`，用下一个继续和`word2[j - 1]`匹配，所以`dp[i][j] = dp[i - 1][j]`；
+ 在`word1`中增加`word2[j - 1]`，所以`dp[i][j] = dp[i][j - 1]`.

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                }
            }
        }
        return dp[m][n];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是非常经典的dp，需要熟练掌握。它是很多字符串转换dp的原型，很多变形都是基于这道题目出的。这道题目的分享到这里，感谢你的支持！
