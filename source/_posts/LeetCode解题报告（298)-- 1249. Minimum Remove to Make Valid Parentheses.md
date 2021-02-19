---
title: LeetCode解题报告（298)-- 1249. Minimum Remove to Make Valid Parentheses
tags:
  - LeetCode
mathjax: true
date: 2021-02-189 21:21:11

---

## Problem

Given a string s of `'('` , `')'` and lowercase English characters. 

Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting *parentheses string* is valid and return **any** valid string.

Formally, a *parentheses string* is valid if and only if:

- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

<!-- more -->

**Example 1:**

```
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2:**

```
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3:**

```
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

**Example 4:**

```
Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
```

**Constraints:**

- `1 <= s.length <= 10^5`
- `s[i]` is one of  `'('` , `')'` and lowercase English letters`.`

------

## Analysis

&emsp;&emsp;题目给出一个包含字符、`(`和`)`的字符串，要求我们通过移除尽可能少的括号，使得这个字符串是valid，对valid的定义就是括号匹配。所以题目的关键就是对括号的处理，遇到字母直接保留就可以了。这里和括号匹配的题目有点类似，但是其实并没有这么复杂，因为这里只有一种类型的括号，所以并不需要用到栈。

&emsp;&emsp;我们先来想invalid的情况有哪些，无非就是三种：

1. `(`比`)` 多；
2. `)`比`(`多；
3. 两个数量相等但是出现的顺序不对。

&emsp;&emsp;所以我们可以先从左到右遍历一遍，统计`(`出现的次数，遇到`)`就减1，如果`(`次数为0，说明`)`数量多出来了，这个时候就需要remove掉；然后再从右往左遍历，这个`(`次数为0，说明`(`数量多出来了，这个时候同样也要remove掉。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        int size = s.size();
        int left = 0;
        string str = "";
        for (int i = 0; i < size; i++) {
            if (s[i] == '(') {
                left++;
            } else if (s[i] == ')') {
                if (left == 0) {
                    continue;
                } else {
                    left--;
                }
            }
            str += s[i];
        }
        
        string result = "";
        int size1 = str.size();
        for (int i = size1 - 1; i >= 0; i--) {
            if (str[i] == '(' && left-- > 0) {
                continue;
            }
            result += str[i];
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的解法展示了一种很常见的解题思路，从两个方向各遍历一遍，这种做法用于计算某数字或字符的出现次数是非常有效的。这道题目的分享到这里，谢谢您的支持！
