---
title: LeetCode解题报告（296)-- 784. Letter Case Permutation
tags:
  - LeetCode
mathjax: true
date: 2021-02-17 11:16:24

---

## Problem

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.

Return *a list of all possible strings we could create*. You can return the output in **any order**.

<!-- more -->

**Example 1:**

```
Input: S = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```

**Example 2:**

```
Input: S = "3z4"
Output: ["3z4","3Z4"]
```

**Example 3:**

```
Input: S = "12345"
Output: ["12345"]
```

**Example 4:**

```
Input: S = "0"
Output: ["0"]
```

**Constraints:**

- `S` will be a string with length between `1` and `12`.
- `S` will consist only of letters or digits.

------

## Analysis

&emsp;&emsp;题目给出了一个包含数字和字母的字符串，我们可以把任何一个字母都转为小写或者大写，要求我们返回所有的组合。思路还是很简单的，就按照题目的要求来。首先我们维护一个`result`的字符串数组，如果遇上数字直接加到每一个字符串的后面；如果遇上字符，分别把大小写加到每一个字符串后面。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<string> letterCasePermutation(string S) {
        vector<string> result{""};
        int size = S.size();
        
        for (int i = 0; i < size; i++) {
            int length = result.size();
            if (S[i] >= '0' && S[i] <= '9') {
                for (string &str: result) {
                    str.push_back(S[i]);
                }
            } else {
                for (int j = 0; j < length; j++) {
                    result.push_back(result[j]);
                    result[j].push_back(tolower(S[i]));
                    result[length + j].push_back(toupper(S[i]));
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
