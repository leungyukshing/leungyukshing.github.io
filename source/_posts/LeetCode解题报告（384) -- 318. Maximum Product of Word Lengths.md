---
title: LeetCode解题报告（384) -- 318. Maximum Product of Word Lengths
tags:
  - LeetCode
mathjax: true
abbrlink: 16266
date: 2021-06-06 13:58:03
---

## Problem

Given a string array `words`, return *the maximum value of* `length(word[i]) * length(word[j])` *where the two words do not share common letters*. If no such two words exist, return `0`.

<!-- more -->

**Example 1:**

```
Input: words = ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16
Explanation: The two words can be "abcw", "xtfn".
```

**Example 2:**

```
Input: words = ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4
Explanation: The two words can be "ab", "cd".
```

**Example 3:**

```
Input: words = ["a","aa","aaa","aaaa"]
Output: 0
Explanation: No such pair of words.
```



**Constraints:**

- `2 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` consists only of lowercase English letters.

------

## Analysis

&emsp;&emsp;这道题给出了一个字符串数组，要求在里面找出两个字符串，使得这两个字符串长度的乘积最大，同时这两个字符串不能有相同的字符。题目的难点是如何快速判断出两个字符串是否拥有相同的字符。比较常规的做法可能是用一个map或者一个长度为26的数组去计算，其实更加快捷的方法应该是使用位运算。一个int就能表示一个单词中含有哪些字符。

&emsp;&emsp;我们首先把`words[i]`中字符出现的情况转为一个int，然后再和`[0, i - 1]`之间的字符串对比，如果两者不存在相同的字符串，则计算一下两者长度的乘积，最后维护一个最大值即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int result = 0;
        int size = words.size();
        vector<int> arr(size, 0);
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < words[i].size(); j++) {
                arr[i] |= (1 << (words[i][j] - 'a'));
            }
            
            for (int j = 0; j < i; j++) {
                if (!(arr[i] & arr[j])) {
                    result = max(result, (int)(words[i].size() * words[j].size()));
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不算大，主要考察的点是用int去表示字符出现的次数，位运算在这一类场景中非常常用，具有运算效率高、节省空间等优点。这道题目的分享到这里，感谢你的支持！
