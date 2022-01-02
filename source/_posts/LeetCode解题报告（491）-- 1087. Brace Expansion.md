---
title: LeetCode解题报告（491）-- 1087. Brace Expansion
mathjax: true
date: 2022-01-01 23:21:25
---

## Problem

You are given a string `s` representing a list of words. Each letter in the word has one or more options.

- If there is one option, the letter is represented as is.
- If there is more than one option, then curly braces delimit the options. For example, `"{a,b,c}"` represents options `["a", "b", "c"]`.

For example, if `s = "a{b,c}"`, the first character is always `'a'`, but the second character can be `'b'` or `'c'`. The original list is `["ab", "ac"]`.

Return all words that can be formed in this manner, **sorted** in lexicographical order.

<!-- more -->

**Example 1:**

```
Input: s = "{a,b}c{d,e}f"
Output: ["acdf","acef","bcdf","bcef"]
```

**Example 2:**

```
Input: s = "abcd"
Output: ["abcd"]
```



**Constraints:**

- `1 <= s.length <= 50`
- `s` consists of curly brackets `'{}'`, commas `','`, and lowercase English letters.
- `s` is guaranteed to be a valid input.
- There are no nested curly brackets.
- All characters inside a pair of consecutive opening and ending curly brackets are different.

---

## Analysis

&emsp;&emsp;题目给出一个字符串`s`，如果是以`{}`包括的字符，说明是可选的，否则就是必须要使用的，要求返回所有的情况。这个BFS了，每次都需要把队列中所有的元素拿出来，添加一个候选字符，只不过这个候选字符有两种情况：

1. 只有一个字符，必选；
2. 以`{}`包括的字符，有多个；

&emsp;&emsp;所以我们处理的方法就是如果处理到`{`，就把括号内的所有字符加入到候选字符；如果处理到的是单个字符，就把单个字符加入到候选字符。然后把队列遍历一遍，把里面的元素都拿出来，加上候选字符重新放到队列中，最后把队列中的字符串都放到vector中即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<string> expand(string s) {
        queue<string> q;
        q.push("");
        
        int i = 0, size = s.size();
        while (i < size) {
            vector<char> next;
            if (s[i] == '{') {
                ++i;
                while (i < size && s[i] != '}') {
                    if (s[i] != ',') {
                        next.push_back(s[i]);
                    }
                    ++i;
                }
            } else {
                next.push_back(s[i]);
            }
            
            int n = q.size();
            for (int k = 0; k < n; ++k) {
                string cur = q.front();
                q.pop();
                
                for (char ch: next) {
                    q.push(cur + ch);
                }
            }
            ++i;
        }
        
        vector<string> result;
        while (!q.empty()) {
            result.push_back(q.front());
            q.pop();
        }
        
        sort(result.begin(), result.end());
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目和decode string有点类似但是完全不同的解法，decode string因为存在嵌套关系，所以用到了stack，而这里并没有嵌套的关系，而是组合关系，所以用BFS。这道题目的分享到这里，感谢你的支持！

