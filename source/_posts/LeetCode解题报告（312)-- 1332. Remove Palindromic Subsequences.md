---
title: LeetCode解题报告（312)-- 1332. Remove Palindromic Subsequences
tags:
  - LeetCode
mathjax: true
abbrlink: 39103
date: 2021-03-10 01:34:28
---

## Problem

Given a string `s` consisting only of letters `'a'` and `'b'`. In a single step you can remove one palindromic **subsequence** from `s`.

Return the minimum number of steps to make the given string empty.

A string is a subsequence of a given string, if it is generated by deleting some characters of a given string without changing its order.

A string is called palindrome if is one that reads the same backward as well as forward.

<!-- more -->

**Example 1:**

```
Input: s = "ababa"
Output: 1
Explanation: String is already palindrome
```

**Example 2:**

```
Input: s = "abb"
Output: 2
Explanation: "abb" -> "bb" -> "". 
Remove palindromic subsequence "a" then "bb".
```

**Example 3:**

```
Input: s = "baabb"
Output: 2
Explanation: "baabb" -> "b" -> "". 
Remove palindromic subsequence "baab" then "b".
```

**Example 4:**

```
Input: s = ""
Output: 0
```

**Constraints:**

- `0 <= s.length <= 1000`
- `s` only consists of letters 'a' and 'b'

------

## Analysis

&emsp;&emsp;题目给出一个字符串，允许的操作是每次移除一个回文子序列，问多少次操作能够把整个字符串清空。一开始想着要怎么找到回文子串，但是看了下题目发现这是道智力题。字符串中只包含两种字符，`a`和`b`，所以情况只有三种：

1. 整个字符串就是回文串，一次操作搞定；
2. 整个字符串不是回文串，首先把所有`a`都去掉，然后把所有`b`都去掉，两次操作搞定；
3. 字符串为空，0次操作。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int removePalindromeSub(string s) {
        if (s.size() == 0) {
            return 0;
        }
        if (s == string(rbegin(s), rend(s))) {
            return 1;
        }
        return 2;
    }
};
```

------

## Summary

&emsp;&emsp;看完分析之后发现这是道智力题，还是提醒自己注意审题哈哈哈哈。这道题目的分享到这里，感谢你的支持！
