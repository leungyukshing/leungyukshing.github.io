---
title: LeetCode解题报告（169）-- 389. Find the Difference
tags:
  - LeetCode
mathjax: true
abbrlink: 28287
date: 2020-09-24 16:45:03
---

## Problem

Given two strings **s** and **t** which consist of only lowercase letters.

String **t** is generated by random shuffling string **s** and then add one more letter at a random position.

Find the letter that was added in **t**.

<!-- more -->

**Example:**

```
Input:
s = "abcd"
t = "abcde"

Output:
e

Explanation:
'e' is the letter that was added.
```

------

## Analysis

&emsp;&emsp;这是一道字符串内找不同的题目，实际上字符串和数字处理的思路都是一样的。比较简单的做法就是用map统计每个字符在`t`出现的次数，然后再遍历`s`，最后剩下多出来的那个字符就是后面插入进去的。

&emsp;&emsp;但是上面这种做法使用了额外的内存空间，其实之前我们处理不同数字的题目有一种思路是可以借鉴的，就是用异或。异或运算的本质就是相同的会抵消，所以只需要对`s`和`t`中的字符都异或，最后剩下的就是多出来的一个，也就是答案了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    char findTheDifference(string s, string t) {
        char ch = 0;
        int size1 = s.size(), size2 = t.size();
        for (int i = 0; i < size1; i++) {
            ch ^= s[i];
        }
        
        for (int i = 0; i < size2; i++) {
            ch ^= t[i];
        }
        
        return ch;
    }
};
```

------

## Summary

&emsp;&emsp;无论是对于字符还是数字，在寻找不同的时候一定要记得有异或这种方法。这道题的分析到这里，谢谢！
