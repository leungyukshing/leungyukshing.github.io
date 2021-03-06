---
title: LeetCode解题报告（310)-- 820. Short Encoding of Words
tags:
  - LeetCode
mathjax: true
date: 2021-03-06 17:06:37

---

## Problem

A **valid encoding** of an array of `words` is any reference string `s` and array of indices `indices` such that:

- `words.length == indices.length`
- The reference string `s` ends with the `'#'` character.
- For each index `indices[i]`, the **substring** of `s` starting from `indices[i]` and up to (but not including) the next `'#'` character is equal to `words[i]`.

Given an array of `words`, return *the **length of the shortest reference string*** `s` *possible of any **valid encoding** of* `words`*.*

<!-- more -->

**Example 1:**

```
Input: words = ["time", "me", "bell"]
Output: 10
Explanation: A valid encoding would be s = "time#bell#" and indices = [0, 2, 5].
words[0] = "time", the substring of s starting from indices[0] = 0 to the next '#' is underlined in "time#bell#"
words[1] = "me", the substring of s starting from indices[1] = 2 to the next '#' is underlined in "time#bell#"
words[2] = "bell", the substring of s starting from indices[2] = 5 to the next '#' is underlined in "time#bell#"
```

**Example 2:**

```
Input: words = ["t"]
Output: 2
Explanation: A valid encoding would be s = "t#" and indices = [0].
```

**Constraints:**

- `1 <= words.length <= 2000`
- `1 <= words[i].length <= 7`
- `words[i]` consists of only lowercase letters.

------

## Analysis

&emsp;&emsp;这道题要求我们对一个单词集合进行编码压缩，从一个集合压缩成一个字符串。对于不同的单词，使用`#`作为分隔符号，如果一个单词是另一个单词的后缀，如`me`是`time`的后缀，此时就可以直接用一个`time`记录即可，但是`ti`不是`time`的后缀，所以不能合并成一个单词。

&emsp;&emsp;第一种做法是对单词集合按照长度排序，先处理长度长的单词，后处理单词短的，这样在处理到短的单词时，就能够查询是否某些更长的单词的后缀了。如果查到是后缀则跳过，如果查到不是后缀，则需要字符串后面加上这个单词。这种做法的正确性是没问题的，但是跑oj时超时了，因为涉及到排序还有很多查询后缀的`find`操作，所以性能不好。

&emsp;&emsp;然后我们需要用一种更加高效的方法，此时考虑使用set。首先把单词都存到一个set中，然后遍历其中每一个单词的所有后缀，如果某个后缀能在set中找到，说明这个后缀的单词是可以合并的，在set中erase掉即可，剩下的就是要添加到答案中了。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        int res = 0;
        unordered_set<string> st(words.begin(), words.end());
        for (string word : st) {
            for (int i = 1; i < word.size(); ++i) {
                st.erase(word.substr(i));
            }
        }
        for (string word : st) res += word.size() + 1;
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目借用了set来提高运行效率，在解决字符串类型题目时，找后缀是性能开销比较大的操作，可以通过一些特殊的数据结构来帮助我们优化代码的性能。这道题目的分享到这里，感谢你的支持！
