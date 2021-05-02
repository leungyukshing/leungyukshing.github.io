---
title: LeetCode解题报告（362) -- 745. Prefix and Suffix Search
tags:
  - LeetCode
mathjax: true
date: 2021-05-02 22:56:28

---

## Problem

Design a special dictionary which has some words and allows you to search the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

- `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
- `f(string prefix, string suffix)` Returns *the index of the word in the dictionary* which has the prefix `prefix` and the suffix `suffix`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `-1`.

<!-- more -->

**Example 1:**

```
Input
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
Output
[null, 0]

Explanation
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".
```

**Constraints:**

- `1 <= words.length <= 15000`
- `1 <= words[i].length <= 10`
- `1 <= prefix.length, suffix.length <= 10`
- `words[i]`, `prefix` and `suffix` consist of lower-case English letters only.
- At most `15000` calls will be made to the function `f`.

------

## Analysis

&emsp;&emsp;题目给出一系列单词，然后要求我们实现一个前后缀的搜索功能，当调用搜索方法时，传入前后缀`prefix`和`suffix`，返回符合要求的单词，如果有多个单词都符合要求，则返回下标最大的。

&emsp;&emsp;首先对每个单词都找出它所有的前缀和后缀，然后用一个非字母的符号把它们两两串联起来，这样的话，在查询的时候，给出前缀和后缀就能快速查找到对应的单词。

&emsp;&emsp;由于在有多个单词都匹配的情况下需要返回下标最大的，所以这里我使用了一个map去做存储，key是匹配的字符串，value是最大的下标。从左往右遍历，这样能够保证匹配字符串相同的情况下，value是最大的下标。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class WordFilter {
public:
    WordFilter(vector<string>& words) {
        int size = words.size();
        for (int i = 0; i < size; i++) {
            int s = words[i].size();
            for (int j = 0; j <= s; j++) {
                for (int k = s; k >= 0; k--) {
                    m[words[i].substr(0, j) + "#" + words[i].substr(k)] = i;                
                }
            }
        }
    }
    
    int f(string prefix, string suffix) {
        return (m.count(prefix + "#" + suffix)) ? m[prefix + "#" + suffix]: -1;
    }
private:
    unordered_map<string, int> m;
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(prefix,suffix);
 */
```

------

## Summary

&emsp;&emsp;这道题目也是比较典型的字符串匹配类型题目，也是通过使用非字母字符的形式，构造出类似正则表达式的字符串，然后存放在map中用于匹配。这道题目的分享到这里，感谢你的支持！
