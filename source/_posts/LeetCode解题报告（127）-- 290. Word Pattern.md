---
title: LeetCode解题报告（127）-- 290. Word Pattern
tags:
  - LeetCode
mathjax: true
abbrlink: 8220
date: 2020-09-07 15:34:04
---

## Problem

Given a `pattern` and a string `str`, find if `str` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `str`.

<!-- more -->

**Example 1:**

```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```

**Example 2:**

```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```

**Example 3:**

```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```

**Example 4:**

```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```

**Notes:**
You may assume `pattern` contains only lowercase letters, and `str` contains lowercase letters that may be separated by a single space.

------

## Analysis

&emsp;&emsp;题目给出了一个pattern和一个字符串（其中的单词以空格隔开），要求我们判断出pattern中的字符与后面字符串中的单词是否满足**一一对应**的关系。一般这种映射的关系，可以很直接地考虑用map来维护。key是pattern中的字符，而value是单词。

&emsp;&emsp;对pattern和单词的遍历是同步进行的，首先检查当前的字符（key）是否已经出现过，如果出现过，则对比value和当前遍历到的单词是否匹配；如果没出现过，还需要检查单词有没有出现过（有可能单词在之前出现过，与其他的key有对应关系）。

------

## Solution

&emsp;&emsp;在建立映射关系的时候，可以使用两个map，同时维护，也可以只使用一个，通过遍历来判断是否存在映射关系。

&emsp;&emsp;这里对字符串里面的单词遍历有个技巧，因为单词是以字符串的方式给出的，如果是python的话按照空格切分会比较方便，但是对于C++来说会有点麻烦。这里使用了输入流`istringstream`，它会一次性读入进内存，然后每次输入会以空格隔开，比较方便。

------

## Code

```c++
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        map<char, string> m;
        int i = 0;
        istringstream in(str);
        int size1 = pattern.size();
        for (string word; in >> word; i++) {
            if (i == size1) {
                // allow words more than pattern
                continue;
            }
            // check pattern
            char ch = pattern[i];
            if (m.count(ch)) {
                // find pattern
                if (m[ch] != word) {
                    return false;
                }
            } else {
                // check word
                for (auto x: m) {
                    if (x.second == word) {
                        return false;
                    }
                }
                m[ch] = word;
            }
        }
        return i == size1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度不算大，但是要很短的时间内快速解决，就要求我们对map、字符串的处理比较熟悉了。这道题的分析到这里，谢谢！
