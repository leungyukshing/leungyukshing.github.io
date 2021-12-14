---
title: LeetCode解题报告（424）-- 1987. Number of Unique Good Subsequences
mathjax: true
date: 2021-12-13 22:39:03
---

## Problem

You are given a binary string `binary`. A **subsequence** of `binary` is considered **good** if it is **not empty** and has **no leading zeros** (with the exception of `"0"`).

Find the number of **unique good subsequences** of `binary`.

- For example, if `binary = "001"`, then all the **good** subsequences are `["0", "0", "1"]`, so the **unique** good subsequences are `"0"` and `"1"`. Note that subsequences `"00"`, `"01"`, and `"001"` are not good because they have leading zeros.

Return *the number of **unique good subsequences** of* `binary`. Since the answer may be very large, return it **modulo** `109 + 7`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

<!-- more -->

**Example 1:**

```
Input: binary = "001"
Output: 2
Explanation: The good subsequences of binary are ["0", "0", "1"].
The unique good subsequences are "0" and "1".
```

**Example 2:**

```
Input: binary = "11"
Output: 2
Explanation: The good subsequences of binary are ["1", "1", "11"].
The unique good subsequences are "1" and "11".
```

**Example 3:**

```
Input: binary = "101"
Output: 5
Explanation: The good subsequences of binary are ["1", "0", "1", "10", "11", "101"]. 
The unique good subsequences are "0", "1", "10", "11", and "101".
```

**Constraints:**

- `1 <= binary.length <= 105`
- `binary` consists of only `'0'`s and `'1'`s.

------

## Analysis

&emsp;&emsp;题目给出一个二进制的字符串，定义了**no leading zeros**的子序列就是**good subsequence**，问一共有多少个good subsequence。题目提示答案要取模，这是提示要使用dp。求subsequence的dp一般都是一维即可，但这里有点特殊，因为要区分出good，我们需要知道当前是以0结尾还是以1结尾，所以需要用二维dp。`dp[i][j]`表示字符串`binary`前`i`个字符以`j`结尾的good subsequence数量。

&emsp;&emsp;状态的范围就是字符串的大小，然后看转移方程。当`binary[i]`为1时，有三种情况：

1. `dp[i - 1][0]`是以0结尾，那么直接加上当前字符就是以1结尾；
2. `dp[i - 1][1]`是以1结尾，直接加上当前字符还是以1结尾；
3. 当前字符可以单独作为一个subsequence。

&emsp;&emsp;所以`dp[i][1] = dp[i - 1][0] + dp[i - 1][1] + 1`。然后我们再看当`binary[i]`为0的情况，有两种：

1. `dp[i - 1][0]`是以0结尾，加上当前字符还是以0结尾；
2. `dp[i - 1][1]`是以1结尾，加上当前字符还是以0结尾。

&emsp;&emsp;需要注意这里就没有了单独的情况，因为我们不考虑0的情况（放到最后考虑即可）。这样做的好处是能够在转移中保证good subsequence。

&emsp;&emsp;更新完最后答案就是`dp[n - 1][0] + dp[n - 1][1]`，如果字符串中出现过0，那么还需要加上1，因为单独的0也是合法的。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numberOfUniqueGoodSubsequences(string binary) {
        int MOD = 1e9 + 7;
        
        int even = 0, odd = 0;
        
        for (char ch: binary) {
            if (ch == '0') {
                even = (even + odd) % MOD;
            } else {
                odd = (even + odd + 1) % MOD;
            }
        }
        
        int result = (even + odd + (binary.find('0') != string::npos)) % MOD;
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较困难的dp，原因在于很难在转移过程中保持good的这个要求。而我这个解法妙就妙在先不考虑单独0的这种情况，而是把这个留到最后一次过考虑，这样如果前面一直是连续的0，那么`dp[i][0]`也就会一直都是0。这道题目的分享到这里，感谢你的支持！
