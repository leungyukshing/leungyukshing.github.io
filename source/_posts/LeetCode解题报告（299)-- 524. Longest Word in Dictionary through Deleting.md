---
title: LeetCode解题报告（299)-- 524. Longest Word in Dictionary through Deleting
tags:
  - LeetCode
mathjax: true
abbrlink: 55099
date: 2021-02-22 22:38:04
---

## Problem

Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

<!-- more -->

**Example 1:**

```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"
```

**Example 2:**

```
Input:
s = "abpcplea", d = ["a","b","c"]

Output: 
"a"
```

**Note:**

1. All the strings in the input will only contain lower-case letters.
2. The size of the dictionary won't exceed 1,000.
3. The length of all the strings in the input won't exceed 1,000.

------

## Analysis

&emsp;&emsp;题目给出一个字符串`s`和一个字典`d`，问通过对`s`中的字符进行删除，能够匹配到的字典中最长的字符串。对`s`来说，删除的字符越少越好，所以对`d`中的字符串来说是越长越好。所以我们可以先对`d`按照字符串的长度进行排序。

&emsp;&emsp;因为顺序也要匹配，所以没有办法像之前那样用map统计出现的次数和种类来判断，只能把字符串遍历一遍匹配。对`d`中的每个单词`str`，我们遍历字符串`s`，然后再用一个下标`idx`指向`str`的开头，每当匹配一个字符，`idx`自增，如果遍历完之后`idx`是`str`的长度，说明整个字符串匹配成功。又因为我们的`d`是排好序的，所以一旦匹配，就是答案，直接return即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        sort(d.begin(), d.end(), [](string a, string b) {
            if (a.size() == b.size()) {
                return a < b;
            }
            return a.size() > b.size();
        });
        
        for (string str: d) {
            int idx = 0;
            for (char c: s) {
                if (idx < str.size() && c == str[idx]) {
                    idx++;
                }
            }
            if (idx == str.size()) {
                return str;
            }
        }
        return "";
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
