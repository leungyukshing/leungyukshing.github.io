---
title: LeetCode解题报告（174）-- 139. Word Break
tags:
  - LeetCode
mathjax: true
date: 2020-09-30 01:26:40

---

## Problem

Given a **non-empty** string *s* and a dictionary *wordDict* containing a list of **non-empty** words, determine if *s* can be segmented into a space-separated sequence of one or more dictionary words.

<!-- more -->

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

**Note:**

- The same word in the dictionary may be reused multiple times in the segmentation.
- You may assume the dictionary does not contain duplicate words.

------

## Analysis

&emsp;&emsp;这道题目一开始做的时候，我以为很简单，打算用一个set存储dict，然后遍历字符串，每次都和dict里面的元素进行匹配，匹配到就进入下一个新的单词。但是这种做法解决不了一种case，例如s = `penapple` ，dict=`["penapple", "pen"]`。这个时候如果按照我上面的思路，会首先匹配到`pen`这个，然后重新开始单词的匹配，`apple`在dict中找不到，所以就会算作是找不到的，而实际上整个单词`penapple`在dict中是有的。为了解决这种方法，就需要不停地对后面进行检查，这样下来复杂度就变得非常高了，很明显是过不了OJ的。

&emsp;&emsp;清醒一下头脑，还是重新从题目开始分析吧。这道题目要求的是某个能否用dict里面的词组成。如果把字符串（由超过一个单词组成）分成两部分，那么这两部分应该也是能够用dict里面的词组成的。有没有发现我这个说法，有点dp的味道了。但是这里的dp并不是直接遍历过去的，而是要按照区间来计算的。

&emsp;&emsp;首先定义`dp[i]`表示`[0, i]`这个区间能否由dict里面的单词组成。然后对于每个`i`，我们需要在这个区间内找到匹配的单词。所以区间`[0, i]`就分成了两块，一部分是`[0,j]`，另一部分是`[j, i]`。因为`j<i`，所以在求`dp[i]`的时候，第一部分的结果是知道的，就是`dp[j]`，而第二部分直接就从dict里面查询了。只有当第一部分为true，且第二部分能在dict中找到时，`dp[i]`就为true了。最后返回结果`dp[size]`即可。

------

## Solution

&emsp;&emsp;为了兼容空字符串的情况，可以把dp数组设置成`size+1`的大小。同时在计算完`dp[i]`的时候，可以直接break掉，开始计算下一个位置的值。

------

## Code

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        int size = s.size();
        vector<bool> dp(size + 1);
        dp[0] = true; // empty string is true
        for (int i = 0; i <= size; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && dict.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[size];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目并不是很典型的dp，但是只要涉及到一些区间、子字符串的题目，dp永远是一个重要的考虑方向，尤其是发现问题可以切分为相似的子问题的时候，dp就一定能帮上忙。这道题的分析到这里，谢谢！
