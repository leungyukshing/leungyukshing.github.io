---
title: LeetCode解题报告（185）-- 316. Remove Duplicate Letters
tags:
  - LeetCode
mathjax: true
date: 2020-10-12 00:40:37


---

## Problem

Given a string `s`, remove duplicate letters so that every letter appears once and only once. You must make sure your result is **the smallest in lexicographical order** among all possible results.

**Note:** This question is the same as 1081: https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/

<!-- more -->

**Example 1:**

```
Input: s = "bcabc"
Output: "abc"
```

**Example 2:**

```
Input: s = "cbacdcbc"
Output: "acdb"
```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

------

## Analysis

&emsp;&emsp;这道题目是字符串类型的变种题，要求我们移除字符串中重复出现的字符，使得去重后的字符串是字典序最小的。按照以往的经验，找重复的字符使用map即可。难点在于怎么保证得到的答案是字典序最小。比如说example1中，当从前往后遍历的时候，当遍历到第一个b和c的时候，我们应该通过map查到，这两个字符在后面还会出现，所以第一次遍历到的时候就不需要加入到答案中。

&emsp;&emsp;通过对example1的分析，我们知道了通过map去找某个字符是否还出现过，然后决定是否加入到答案中，但是还是没有解决字典序最小的问题。以example2为例，可以看到有很多个`c`，但是只有`a`后面这个`c`才是真正添加到答案中的，后面的两个`c`都不用考虑。所以这里要对字典序进行判断，因为是一个一个字符插入的，所以和后面的一个字符比较不方便，这里采用的思路就是和当前答案中最后一个字符（之前插入过的）做比较，如果答案中最后一个字符后续还会出现，而且比当前的这个字符要大，则说明这个位置应该是当前这个字符的。

------

## Solution

&emsp;&emsp;使用map来统计字符出现的次数，同时还要用一个数组标记每个字符是否被加入到答案中，后者是用来跳过重复的字符的。

------

## Code

```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        map<char, int> m;
        int visited[256] = {0};
        int size = s.size();
        for (int i = 0; i < size; i++) {
            m[s[i]]++;
        }
        
        string result = "0";
        for (auto ch: s) {
            m[ch]--;
            if (visited[ch]) {
                continue;
            } else {
                while (ch < result.back() && m[result.back()]) {
                    visited[result.back()] = 0;
                    result.pop_back();
                }
                result += ch;
                visited[ch] = 1;
            }
            // cout << result << endl;
        }
        return result.substr(1);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点是加了字典序的限制后，在判断的时候要考虑前后的大小关系，而对于重复的判断，还是采用map的方法。这道题目的分享到这里，谢谢！
