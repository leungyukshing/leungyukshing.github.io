---
title: LeetCode解题报告（416) -- 1593. Split a String Into the Max Number of Unique Substrings
mathjax: true
date: 2021-12-12 16:11:12
---

## Problem

Given a string `s`, return *the maximum number of unique substrings that the given string can be split into*.

You can split string `s` into any list of **non-empty substrings**, where the concatenation of the substrings forms the original string. However, you must split the substrings such that all of them are **unique**.

A **substring** is a contiguous sequence of characters within a string.

<!-- more -->

**Example 1:**

```
Input: s = "ababccc"
Output: 5
Explanation: One way to split maximally is ['a', 'b', 'ab', 'c', 'cc']. Splitting like ['a', 'b', 'a', 'b', 'c', 'cc'] is not valid as you have 'a' and 'b' multiple times.
```

**Example 2:**

```
Input: s = "aba"
Output: 2
Explanation: One way to split maximally is ['a', 'ba'].
```

**Example 3:**

```
Input: s = "aa"
Output: 1
Explanation: It is impossible to split the string any further.
```

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lower case English letters.

------

## Analysis

&emsp;&emsp;题目给出一个字符串，要求把字符串切割成不同的子串（连续的），要求找到最多不同的子串，而我们只需要找到这个数量就行。虽然不需要找到所有可能的答案，但这依然是一道很适合使用backtracking的题目。

&emsp;&emsp;按照backtracking的套路，我们来看看回溯的参数。因为我们不需要返回所有可能的答案，所以参数中不需要有当前字符串，只需要一个`s`表示原始的字符串，以及一个`idx`表示当前遍历到的位置即可。

&emsp;&emsp;然后我们来看终止条件。因为我们要记录的是不同的子串，所以使用一个`unordered_set`就可以了，这个set的大小就是答案。我们只需要在全局维护一个最大值就是答案。同时，当`idx == s.size()`时，说明已经遍历完字符串了，此时直接return即可。

&emsp;&emsp;最后看回溯逻辑。因为子串必须连续，所以函数中需要维护一个`temp`变量，不断添加字符进去。特别注意的是，这里的回溯并不是撤回当前字符。原因是要保证连续，撤回了当前字符就不连续了。这里的撤回指的是是否放入到set中。以example2为例，假如集合中已经有了`["a","b"]`这两个字符串，在遇到第三个字符`a`时，显然不符合题意，于是返回到`b`中，此时并不是字符串中把`b`撤回，而应该是在集合中把`b`撤回，然后在当前这个字符串后面加上`a`得到`ba`，然后再加入到集合中。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int maxUniqueSplit(string s) {
        result = 0;
        helper(s, 0);
        return result;
    }
private:
    int result;
    unordered_set<string> st;
    void helper(string s, int idx) {
        result = max(result, (int)st.size());
        if (idx == s.size()) {
            return;
        }
        string temp = "";
        for (int i = idx; i < s.size(); i++) {
            temp += s[i];
            if (!st.count(temp)) {
                st.insert(temp);
                helper(s, i + 1);
                st.erase(temp);
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目也是非常典型的回溯框架，难点在于撤回操作的选择。因为要求子串，所以撤回的不是添加的当前字符，而是在集合中撤回刚添加的字符串。这道题目的分享到这里，感谢你的支持！
