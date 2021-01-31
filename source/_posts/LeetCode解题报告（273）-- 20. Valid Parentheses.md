---
title: LeetCode解题报告（273）-- 20. Valid Parentheses
tags:
  - LeetCode
mathjax: true
date: 2021-01-31 22:41:26

---

## Problem

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

<!-- more -->

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```
Input: s = "(]"
Output: false
```

**Example 4:**

```
Input: s = "([)]"
Output: false
```

**Example 5:**

```
Input: s = "{[]}"
Output: true
```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

------

## Analysis

&emsp;&emsp;这是一道很经典的stack应用题目了，利用栈的特性做括号匹配校验。遇到左边的括号就入栈，遇到右边的括号就和栈顶做匹配，一旦不符合就return false。特别需要注意两种情况：一个是遍历过程中栈已经为空而且出现右边的括号；另一个是遍历完所有后栈还有元素。这两种情况都是无法匹配的，要return false。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool isValid(string s) {
        int size = s.size();
        stack<char> st;
        for (int i = 0; i < size; i++) {
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
                st.push(s[i]);
            } else {
                if (st.empty()) {
                    return false;
                }
                char ch = st.top();
                st.pop();
                if (ch == '(' && s[i] == ')' || ch == '[' && s[i] == ']' || ch == '{' && s[i] == '}') {
                    continue;
                } else {
                    return false;
                }
            }
        }
        return st.empty();
    }
};
```

------

## Summary

&emsp;&emsp;这道题是比较经典的stack题目，这道题目其实就是算术表达式的实现原理，掌握这道题目对理解stack有很好的帮助。这道题这道题目的分享到这里，谢谢您的支持！
