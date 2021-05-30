---
title: LeetCode解题报告（378) -- 1048. Longest String Chain
tags:
  - LeetCode
mathjax: true
date: 2021-05-30 15:06:17

---

## Problem

Given a list of words, each word consists of English lowercase letters.

Let's say `word1` is a predecessor of `word2` if and only if we can add exactly one letter anywhere in `word1` to make it equal to `word2`. For example, `"abc"` is a predecessor of `"abac"`.

A *word chain* is a sequence of words `[word_1, word_2, ..., word_k]` with `k >= 1`, where `word_1` is a predecessor of `word_2`, `word_2` is a predecessor of `word_3`, and so on.

Return the longest possible length of a word chain with words chosen from the given list of `words`.

<!-- more -->

**Example 1:**

```
Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chain is "a","ba","bda","bdca".
```

**Example 2:**

```
Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5
```



**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 16`
- `words[i]` only consists of English lowercase letters.

------

## Analysis

&emsp;&emsp;这道题是单词链问题，题目给出一堆单词，形成链的条件是：后一个单词比前一个单词长度多1，而且仅有一个字符不同。题目要求的是最长单词链长度。

&emsp;&emsp;首先求最长的长度，这种就可以往dp上面靠。定义`dp[i]`为`words[i]`的单词链最长长度。如果`words[j]`是`words[i]`的predecessor，状态转移方程为`dp[i] = max(dp[i], dp[j] + 1)`。所以重点就在于如何判断是predecessor。

&emsp;&emsp;首先是长度，前一个单词的长度假设为`x`，则后一个单词必须是`x + 1`。长度满足要求后，再检查不同的字符是否只有一个，如果都满足则说明是predecessor。

&emsp;&emsp;由于这里检查对长度有限制，所以我们先对所有的单词按照长度进行排序，然后从左到右开始遍历，先遍历长度较短的。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int longestStrChain(vector<string>& words) {
        sort(words.begin(), words.end(), [](string &A, string &B) {
            return A.size() < B.size();
        });
        int size = words.size();
        
        vector<int> dp(size, 1);
        
        int result = 1;
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < i; j++) {
                if (words[i].size() == words[j].size()) {
                    break;
                }
                if (words[i].size() - 1 > words[j].size()) {
                    continue;
                }
                if (isPredecessor(words[j], words[i])) {
                    dp[i] = max(dp[i], dp[j] + 1);
                    result = max(result, dp[i]);
                }
            }
        }
        return result;
    }
private:
    bool isPredecessor(string A, string B) {
        bool flag = false;
        int idx1 = 0, idx2 = 0;
        while (idx1 < A.size() && idx2 < B.size()) {
            if (A[idx1] == B[idx2]) {
                idx1++;
                idx2++;
            } else if (!flag) {
                flag = true;
                idx2++;
            } else {
                return false;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是dp经典题之一，dp的定义和状态转移都比较简单，难点是dp转移时的判断逻辑。这道题目的分享到这里，感谢你的支持！
