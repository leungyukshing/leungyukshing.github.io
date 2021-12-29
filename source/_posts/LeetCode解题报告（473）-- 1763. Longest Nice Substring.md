---
title: LeetCode解题报告（473）-- 1763. Longest Nice Substring
mathjax: true
date: 2021-12-29 16:23:51
---

## Problem

A string `s` is **nice** if, for every letter of the alphabet that `s` contains, it appears **both** in uppercase and lowercase. For example, `"abABB"` is nice because `'A'` and `'a'` appear, and `'B'` and `'b'` appear. However, `"abA"` is not because `'b'` appears, but `'B'` does not.

Given a string `s`, return *the longest **substring** of `s` that is **nice**. If there are multiple, return the substring of the **earliest** occurrence. If there are none, return an empty string*.

<!-- more -->

**Example 1:**

```
Input: s = "YazaAay"
Output: "aAa"
Explanation: "aAa" is a nice string because 'A/a' is the only letter of the alphabet in s, and both 'A' and 'a' appear.
"aAa" is the longest nice substring.
```

**Example 2:**

```
Input: s = "Bb"
Output: "Bb"
Explanation: "Bb" is a nice string because both 'B' and 'b' appear. The whole string is a substring.
```

**Example 3:**

```
Input: s = "c"
Output: ""
Explanation: There are no nice substrings.
```



**Constraints:**

- `1 <= s.length <= 100`
- `s` consists of uppercase and lowercase English letters.

---

## Analysis

&emsp;&emsp;题目给出一个字符串`s`，定义nice substring是substring中每个字符的大小写都出现过，要求返回最长的nice substring。字符串题目中substring的题目一般有几种解法，一种是滑动窗口，另一种是分治。这里滑动窗口其实并不是很适合，原因是我们很难定义收窄窗口的条件，我们要维护窗口里面有什么字符，还要知道出现的顺序。所以这里我用的是分治，首先我们要记录字符串中出现过的所有字符，这样我们才能判断出某个字符的大小写是否都出现过，只有满足要求的字符我们才会考虑，如果不满足，说明substring肯定不包含这个字符。

&emsp;&emsp;先看分治的解法，对于不满足要求的字符，递归调用其左、右子串，并且返回长度较大的那个，最后就是结果。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    string longestNiceSubstring(string s) {
        int n = s.size();
        if (n < 2) {
            return "";
        }
        
        unordered_set<char> st;
        for (char ch: s) {
            st.insert(ch);
        }
        
        for (int i = 0; i < n; i++) {
            if (st.count(toupper(s[i])) && st.count(tolower(s[i]))) {
                continue;
            }
            
            // discard s[i]
            string str1 = longestNiceSubstring(s.substr(0, i));
            string str2 = longestNiceSubstring(s.substr(i + 1));
            return str1.size() >= str2.size() ? str1 : str2;
        }
        return s;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目告诉我们数组中subarray不一定都是用sliding window，因为这里window收窄条件并不容易写出来，所以更好的做法应该是采用分治，分治的好处在于它省去了从左到右一直维护window信息的麻烦，只需要focus在当前的子串。这道题目的分享到这里，感谢你的支持！

