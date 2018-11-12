---
title: LeetCode解题报告（21）-- 10. Regular Expression Matching
tags:
  - LeetCode
abbrlink: 39306
date: 2018-10-16 14:06:52
---
## Problem
Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**
  + `s` could be empty and contains only lowercase letters `a-z`.
  + `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.
<!-- more -->

**Example 1**:
```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```

**Example 2**:
```
Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3**:
```
Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4**:
```
Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5**:
```
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```

## Analysis
&emsp;&emsp;这是一题关于正则表达式匹配的问题，使用python可以非常快速地解决，而使用C++则需要时间思考了。首先我们来看匹配的方式，大致分为三种：
  + 具体的字符匹配，即`s`和`p`中含有相同的字符，匹配成功。
  + `.`匹配，`.`可以匹配任意的字母（单个）。
  + `*`匹配，这是最难的地方，考虑到可以匹配0个或多个相同的字符。

&emsp;&emsp;确认好匹配的方式后，我们来确定匹配的思路：**匹配成功就删除**，最后如果都为空或者只剩一个字符且匹配的话，则算成功。那么具体如何实现呢？循环还是迭代呢？因为这是一个判断正确与否的题目，因此使用递归更加便于理解。对于空字符串、单个字符匹配我们具体讨论，对于长度大于等于2的，我们需要考虑`*`的情况：
  + 如果`p`的第二个字符不是`*`，那么我们可以匹配第一个字符，然后同时删掉`s`和`p`的第一个字符，递归。
  + 如果`p`的第二个字符是`*`，那么就需要对`s`中的第一个字符不断进行匹配，这里又分为两种情况：
    + 出现0次：直接删除`p`中前两个字符，递归
    + 出现多次：匹配`s`中第一个字符，若成功则删除`s`中第一个字符，递归。（注意，不需要删除`p`中的字符）

&emsp;&emsp;整个过程只要有一个地方无法匹配，则全局会返回`false`，这就是递归的好处。在删除字符的时候可以使用`substr()`，比较方便。

## Solution
&emsp;&emsp;按照上述思路进行编程问题不大。特别需要注意的是，在比较字符前必须先确认字符串不为空，否则会出现非法访问的错误！
## Code
```C++
class Solution {
public:
    bool isMatch(string s, string p) {
        if (p.empty())
            return s.empty();
        else if (s.size() == 1 && p.size() == 1 && (s == p || p == ".")) {
            return true;
        }
        else if (p[1] != '*') {
            if (s.empty()) {
                return false;
            }
            return (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
        }
        else {
            return isMatch(s, p.substr(2)) || (!s.empty() && (s[0] == p[0] || p[0] == '.')) && isMatch(s.substr(1), p);
        }
    }
};
```
**运行时间**：约64ms，超过23.62%的CPP代码。

## Summary
&emsp;&emsp;我以往没有碰到过类似的题，因此在分析中花了不少时间，也走了不少弯路（一开始打算用遍历的方法，发现很复杂而且很乱）。最后使用了递归，整个思路也就很明确了，也很利于编程。我的体会是**对于一些看似复杂但有一定规律的题目，不妨使用递归的思路去求解**。这道题的分析就到这里，谢谢您的支持！
