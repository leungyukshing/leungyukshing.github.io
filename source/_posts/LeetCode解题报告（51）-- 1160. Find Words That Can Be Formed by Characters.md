---
title: LeetCode解题报告（51）-- 1160. Find Words That Can Be Formed by Characters
tags:
  - LeetCode
abbrlink: 55144
date: 2019-09-18 01:22:42
---

## Problem

You are given an array of strings `words` and a string `chars`.

A string is *good* if it can be formed by characters from `chars` (each character can only be used once).

Return the sum of lengths of all good strings in `words`.

<!-- more -->

**Example 1:**

```
Input: words = ["cat","bt","hat","tree"], chars = "atach"
Output: 6
Explanation: 
The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.
```

**Example 2:**

```
Input: words = ["hello","world","leetcode"], chars = "welldonehoneyr"
Output: 10
Explanation: 
The strings that can be formed are "hello" and "world" so the answer is 5 + 5 = 10.
```

**Note:**

1. `1 <= words.length <= 1000`
2. `1 <= words[i].length, chars.length <= 100`
3. All strings contain lowercase English letters only.

------

## Analysis

&emsp;&emsp;这道题是关于字符串匹配的，不过与常规的匹配不同，这里的匹配是给出一个字典`chars`，需要在其中不重复地使用字符来匹配。既然是不重复且不按顺序，我们可以充分利用vector的特点来做这道题。vector提供了`find`和`erase`函数能够很好地满足我们做题的需要。

------

## Solution

&emsp;&emsp;对于每一个单词都进行匹配，匹配过程是使用`find` 在字典中查找，如果匹配到，则删除该字符（题目要求不能重复使用），如果没有匹配到，则结束该单词的匹配。

------

## Code

```c++
class Solution {
public:
    int countCharacters(vector<string>& words, string chars) {
        int result = 0;
        int size = words.size();
        for (int i = 0; i < size; i++) {
            vector<char> ch;
            ch.resize(chars.size());
            ch.assign(chars.begin(),chars.end());
            
            string word = words[i];
            int s = word.size();
            int j = 0;
            for (j = 0; j < s; j++) {
                vector<char>::iterator it;
                it = find(ch.begin(), ch.end(), word[j]);
                if (it != ch.end()) {
                    // Found
                    ch.erase(it);
                }
                else {
                    break;
                }
            }
            if (j == s) {
                result += s;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道字符串匹配的题目，匹配不是顺序匹配，所以要充分利用到vector的特点。算法思路并没有很难的地方，题目的价值在于让大家能够熟悉使用`find`和`erase`。分享就到这里，欢迎大家转发、评论， 谢谢你的支持！
