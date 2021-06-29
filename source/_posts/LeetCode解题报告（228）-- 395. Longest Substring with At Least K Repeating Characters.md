---
title: >-
  LeetCode解题报告（228）-- 395. Longest Substring with At Least K Repeating
  Characters
tags:
  - LeetCode
mathjax: true
abbrlink: 5941
date: 2020-11-27 01:35:02
---

## Problem

Given a string `s` and an integer `k`, return *the length of the longest substring of* `s` *such that the frequency of each character in this substring is greater than or equal to* `k`.

<!-- more -->

**Example 1:**

```
Input: s = "aaabb", k = 3
Output: 3
Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input: s = "ababbc", k = 2
Output: 5
Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 105`

------

## Analysis

&emsp;&emsp;这道题是字符串类型的子串类题目，这类题目的难点通常是计算复杂度很高。原因是子串的长度不确定，全部遍历一遍的话需要大量的计算。同时这里还提醒一下，子串（substring）和子序列（subsequence）是不同的，前者必须是连续的，而后者是可以不连续的。后者的解法以dp为主，前者的解法则没有特别好的规律。

&emsp;&emsp;先来看看题目的要求，要找到一个子串，子串内每个元素出现的次数都超过`k`。统计出现的次数是比较容易的，无论是用map还是用vector都很容易实现，比较难的是如何去找到子串。有几种常用的思路：例如，按照子串的长度。比如从长度为1开始找，然后找长度为2的，接下来找长度为3的，以此类推。而对于只包含字母的字符串来说，**按照字母的种类数来划分也是一种常见的做法**。原因是无论原来的字符串有多长，字母的种类就只有26个，所以这个是一个有限而且比较小的数。

&emsp;&emsp; 这道题目就是利用这种按照字母种类数来划分，我们先找只包含1种字母的子串，然后找包含2种字母的子串，以此类推。在种类数确定的情况下如何遍历呢？这里又涉及到了字符串类型题目一个很重要的技巧——**滑动窗口**。`right`每向右滑动一次，检查是否引入了新的字母数，如果引入了新的字母数导致了当前子串的字母种类数超出了限制，`left`指针就需要同时右移，确保字母种类数是一致的。滑动窗口的好处是可以用一个数据结构维护字母出线的次数，判断是否满足题目的要求就比较方便了，全局再维护一个满足题目要求的长度的最大值，每次都检查更新即可。

------

## Solution

&emsp;&emsp;统计次数一般来说会用map，但是字母是连续的所以用vector也是一样的。需要注意窗口的滑动要保证子串中字母的种类数没有超出限制。

------

## Code

```c++
class Solution {
public:
    int longestSubstring(string s, int k) {
        int result = 0, size = s.size();
        for (int count = 1; count <= 26; count++) {
            vector<int> dict(26);
            int left = 0, right = 0, uniqueCount = 0;
            while (right < size) {
                bool valid = true;
                if (dict[s[right++] - 'a']++ == 0) {
                    uniqueCount++;
                }
                while (uniqueCount > count) {
                    if (--dict[s[left++] - 'a'] == 0) {
                        uniqueCount--;
                    }
                }
                // check dict
                for (int j  = 0; j < 26; j++) {
                    if (dict[j] > 0 && dict[j] < k) {
                        valid = false;
                    }
                }
                if (valid) {
                    result = max(result, right - left);
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是难度比较大的字符串子串类题目，结合了词频统计、滑动窗口、子串等知识点，是一道比较考验综合能力的好题。这道题目的分享到这里，谢谢！
