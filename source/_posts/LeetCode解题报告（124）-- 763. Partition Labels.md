---
title: LeetCode解题报告（124）-- 763. Partition Labels
tags:
  - LeetCode
mathjax: true
abbrlink: 12429
date: 2020-09-04 15:21:28
---

## Problem

A string `S` of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

<!-- more -->

**Example 1:**

```
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
```

**Note:**

- `S` will have length in range `[1, 500]`.
- `S` will consist of lowercase English letters (`'a'` to `'z'`) only.

------

## Analysis

&emsp;&emsp;这道题目是一道字符串分割的题目，给定一个字符串，要求切成尽可能多的子串，要求每个字母只能出现在其中的一个子串中。我们首先想一下是什么决定一个字母在哪个子串？**是它最后一次出现的位置**决定了它在哪个子串，所以我们需要先记录下每个字母最后一次出现的位置。然后从头开始遍历字符串，我们记录下区间的起始位置和结束位置（结束的意思是该区间的结束位置是该子串包含的某个字母的最后一次出现的下标），每遍历到一个字母，就使用它最后一次出现的位置去更新结束位置，当遍历到的下标等于这个结束的位置，说明这个区间就完整了，然后就可以开始下一个区间的计算了。

------

## Solution

&emsp;&emsp;用两个下标记录区间的起始位置和结束位置，然后用map记录每个字母和它对应的最后一次出现的下标。

------

## Code

```c++
class Solution {
public:
    vector<int> partitionLabels(string S) {
        vector<int> result;
        map<char, int> m;
        
        int size = S.size();
        for (int i = 0; i < size; i++) {
            m[S[i]] = i;
        }
        
        int start = 0, end = 0;
        for (int i = 0; i < size; i++) {
            end = max(end, m[S[i]]);
            if (end == i) {
                result.push_back(end - start + 1);
                start = end + 1;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题的分析到这里，谢谢！
