---
title: LeetCode解题报告（133）-- 58. Length of Last Word
tags:
  - LeetCode
mathjax: true
abbrlink: 26473
date: 2020-09-15 22:18:31
---

## Problem

Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word (last word means the last appearing word if we loop from left to right) in the string.

If the last word does not exist, return 0.

**Note:** A word is defined as a **maximal substring** consisting of non-space characters only.

<!-- more -->

**Example 1:**

```
Input: "Hello World"
Output: 5
```

------

## Analysis

&emsp;&emsp;这道题要求的是最后一个单词的长度。实际上麻烦的地方就是处理前后的空格。有两种做法：第一种是去掉后面的空格，然后从后往前遍历，遇到空格或者到头就停止；第二种是直接从后面开始遍历，当遇到第一个字符的时候认为开始计数，当遇到空格的时候就停止。

------

## Solution

&emsp;&emsp;根据上述两种思路，有不同的实现方法：

1. 第一种实际上就是实现一个`trim()`，可以借助STL的`find_first_not_of()`和`find_last_not_of()`来删除首尾的空格；
2. 第二种用两个标志来记录，一个是`hasSpace`，用来跳过后面的空格，另一个是`hasStart`，用来记录是否已经开始计数了。

------

## Code

第一种方法因为调用了STL，可能耗时和空间占用比较多，不过为了分享`trim()`这里也一并给出；第二种方法的耗时更少，空间占用也较少。

方法1：

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        string str = trim(s);
        int size = str.size();
        int len = 0;
        for (int i = size - 1; i >= 0; i--) {
            if (str[i] == ' ') {
                break;
            } else {
                len++;
            }
        }
        return len;
    }
private:
    string& trim(string& str) {
        if (str.empty()) {
            return str;
        }
        str.erase(0, str.find_first_not_of(' '));
        str.erase(str.find_last_not_of(' ') + 1);
        return str;
    }
};
```

方法2：

```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        bool hasSpace = false;
        bool hasStart = false;
        int size = s.size();
        int len = 0;
        for (int i = size - 1; i >= 0; i--) {
            if (s[i] == ' ') {
                hasSpace = true;
            }
            if (!hasStart && s[i] != ' ') {
                hasStart = true;
            }
            if (hasStart && s[i] != ' ') {
                len++;
            }
            if (hasSpace && hasStart && s[i] == ' ') {
                break;
            }
        }
        return len;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本身是非常简单的，就是最基本的字符串遍历，值得总结的点是用C++实现了trim方法，这在其他更加难的题目中可能是比较有用的。这道题的分析到这里，谢谢！
