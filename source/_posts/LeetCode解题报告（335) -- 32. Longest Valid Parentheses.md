---
title: LeetCode解题报告（335) -- 32. Longest Valid Parentheses
tags:
  - LeetCode
mathjax: true
abbrlink: 32784
date: 2021-04-10 01:57:29
---

## Problem

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

<!-- more -->

**Example 1:**

```
Input: s = "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()".
```

**Example 2:**

```
Input: s = ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()".
```

**Example 3:**

```
Input: s = ""
Output: 0
```

**Constraints:**

- `0 <= s.length <= 3 * 104`
- `s[i]` is `'('`, or `')'`.

------

## Analysis

&emsp;&emsp;题目给出一个包含左右括号的字符串，要求判断这个字符串中最长的合法的子串长度。因为只有一种括号，所以不需要用栈来判断，用两个值记录左右括号出现的次数就能判断是否匹配上。从左往右遍历，正常来说先是左括号的数量大于右括号的数量，然后当恰好匹配的时候，两者相等，一旦发现右括号的数量较大，则说明已经遍历过的字符串不是有效的。

&emsp;&emsp;按照上面的解法确实可以cover大多数的场景，但是对于题目给出的Example1，好像就不行了。因为左括号的数量是2，右括号的数量是1，两者不相等，所以无法确认合法子串的长度，这种情况我们需要怎么解决呢？

&emsp;&emsp;解决的方法其实非常简单，从右往左再做一遍相同的操作即可。但是判断的条件关系恰好相反，先是右括号的数量大于左括号的数量，然后当两者数量相等时，说明恰好匹配，一旦发现左括号的数量较大，则重置。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int left = 0, right = 0;
        int size = s.size(), result = 0;
        for (int i = 0; i < size; i++) {
            s[i] == '(' ? left++: right++;
            if (left == right) {
                result = max(result, left * 2);
            } else if (right > left) {
                left = right = 0;
            }
        }
        
        left = right = 0;
        for (int i = size - 1; i >=0; i--) {
            s[i] == '(' ? left++: right++;
            if (left == right) {
                result = max(result, left * 2);
            } else if (left > right) {
                left = right = 0;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道比较简单的括号匹配题目，这里主要利用了两遍遍历来解决。这道题目的分享到这里，感谢你的支持！
