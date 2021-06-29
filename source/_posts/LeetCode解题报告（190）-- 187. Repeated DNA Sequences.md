---
title: LeetCode解题报告（190）-- 187. Repeated DNA Sequences
tags:
  - LeetCode
mathjax: true
abbrlink: 52750
date: 2020-10-18 21:24:40
---

## Problem

All DNA is composed of a series of nucleotides abbreviated as `'A'`, `'C'`, `'G'`, and `'T'`, for example: `"ACGAATTCCG"`. When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

<!-- more -->

**Example 1:**

```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
Output: ["AAAAACCCCC","CCCCCAAAAA"]
```

**Example 2:**

```
Input: s = "AAAAAAAAAAAAA"
Output: ["AAAAAAAAAA"]
```

**Constraints:**

- `0 <= s.length <= 105`
- `s[i]` is `'A'`, `'C'`, `'G'`, or `'T'`.

------

## Analysis

&emsp;&emsp;这是一道字符串匹配的题目，题目要求的是匹配出所有重复出现的，长度为10的字符串。这类题目一般来说都是要顺序遍历一次，优化的点就在字符串匹配的部分。匹配的前提是要存下来第一次出现的内容，这样后面再次出现的时候才能知道是重复出现的。之前通常的做法是直接存成一个字符串，虽然字符串的存储是比较方便的，但是对字符串头尾操作是比较困难的。因为要匹配10个字符，相当于一个窗口，所以待匹配的字符串是需要不断删掉开头，插入结尾。

&emsp;&emsp;优化这种存储方法最好用位来存储。因为题目给出的字符串只会含有A、C、T、G四种字符，我们就可以利用编码的特点来做。从它们的ASCII码入手，A（0100 0**001**）、C（0100 0**011**）、G（0100 0**111**）、T（0101 0**100**），可以后三位都是不一样的，所以说我们可以直接通过后三位来区分这四个字符。对于长度为10的字符串，一共就需要30位来存储，所以用一个int变量（32位）就可以存储了。匹配的时候只需要直接比对数字的值就可以了。

------

## Solution

&emsp;&emsp;为了方便取30位，我们可以用一个mask取后27位，然后再左移三位，就能拿到30位了。每次取到一个新的字符时，为了拿到后三位，要用`111`做与运算拿出来。

&emsp;&emsp;当窗口从头到尾移动的时候，我们只需要每次用mask取30位出来，然后把最后三位替换成新的即可。这样最左边的3位就没掉了，最右的三位就进来了。

------

## Code

```c++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        if (s.size() <= 10) return res;
        int mask = 0x7ffffff, cur = 0;
        unordered_map<int, int> m;
        for (int i = 0; i < 9; ++i) {
            cur = (cur << 3) | (s[i] & 7);
        }
        for (int i = 9; i < s.size(); ++i) {
            cur = ((cur & mask) << 3) | (s[i] & 7);
            if (m.count(cur)) {
                if (m[cur] == 1) res.push_back(s.substr(i - 9, 10));
                ++m[cur]; 
            } else {
                m[cur] = 1;
            }
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目思路没有很复杂，就是简单的遍历和匹配。有意思的点是在用bit来存储字符串，通过简单的编码操作，用int存长度为10的字符串。这道题目的分享到这里，谢谢！
