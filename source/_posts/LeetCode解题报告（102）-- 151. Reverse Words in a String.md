---
title: LeetCode解题报告（102）-- 151. Reverse Words in a String
tags:
  - LeetCode
mathjax: true
abbrlink: 58828
date: 2020-07-20 23:10:17
---

## Problem

Given an input string, reverse the string word by word.

<!-- more -->

**Example 1:**

```
Input: "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Note:**

- A word is defined as a sequence of non-space characters.
- Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
- You need to reduce multiple spaces between two words to a single space in the reversed string.

------

## Analysis

&emsp;&emsp;这道题目是字符串翻转的经典题目，总体的思路就是整体翻转一遍，然后每个小部分再翻转一次。难点主要有两个，一个是要跳过两个部分之间多余的空格；二是要计算出小部分的起始位置和终止位置。

------

## Solution

&emsp;&emsp;都在原字符串上进行，使用一个`storeIndex`来记录当前存储的下标，然后每到一个新的部分就先加入一个空格。对于每一个小部分，先把里面的内容赋值，通过`i`和`j`两个下标，一个记录起始位置，一个记录终止位置，最后计算出小部分的长度。然后通过`reverse()`来进行翻转。最后不要忘了`resize()`一下，释放掉原字符串多余的空间。

------

## Code

```c++
class Solution {
public:
    string reverseWords(string s) {
        int storeIndex = 0, n = s.size();
        reverse(s.begin(), s.end());
        for (int i = 0; i < n; ++i) {
            if (s[i] != ' ') {
                if (storeIndex != 0) s[storeIndex++] = ' ';
                int j = i;
                while (j < n && s[j] != ' ') s[storeIndex++] = s[j++];
                reverse(s.begin() + storeIndex - (j - i), s.begin() + storeIndex);
                i = j;
            }
        }
        s.resize(storeIndex);
        return s;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度不大，之前在博客也有类似的题解，在这里是再一次把思路理清楚，善用`reverse()`方法，能够提高coding的速度。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
