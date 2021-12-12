---
title: LeetCode解题报告（411) -- 301. Remove Invalid Parentheses
mathjax: true
date: 2021-12-11 16:54:06
---

## Problem

Given a string `s` that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.

Return *all the possible results*. You may return the answer in **any order**.

<!-- more -->

**Example 1:**

```
Input: s = "()())()"
Output: ["(())()","()()()"]
```

**Example 2:**

```
Input: s = "(a)())()"
Output: ["(a())()","(a)()()"]
```

**Example 3:**

```
Input: s = ")("
Output: [""]
```

**Constraints:**

- `1 <= s.length <= 25`
- `s` consists of lowercase English letters and parentheses `'('` and `')'`.
- There will be at most `20` parentheses in `s`.

------

## Analysis

&emsp;&emsp;这是一道和括号相关的题目，题目给出一个字符串包含了字母和括号，要求通过移除最少的括号使得字符串中的括号匹配，并返回所有可能的结果。首先，因为题目要求返回所有可能的结果，所以这一看上去就往回溯的方向靠了。然后我们来想一下，怎么样才是移除最少的括号？

&emsp;&emsp;回想一下我们怎么去验证括号是否匹配，我们从左到右遍历，统计左括号出现的次数，左括号出现一次`left`加一，右括号出现一次则`left`减1。这里我们采用同样的思路，因为我们不单止会移除左括号，还会移除右括号，所以当`left == 0`并且遇到右括号时，`right`加一。遍历完之后，`left`代表着多余的左括号的数量，`right`代表多余右括号的数量。然后我们就可以开始回溯了。

&emsp;&emsp;回溯的参数包括有字符串`s`，当前下标`idx`，结果`result`，以及`left`和`right`两个变量。需要注意，这里的字符串是包含字母的，但是字母是不会影响括号匹配，所以我们只处理当前字符是括号的情况。当遇到左括号并且`left`大于0时，说明我可以通过删除当前字符使得字符串向合法的方向更进一步；当遇到右括号并且`right`大于0时，说明我可以通过删除当前字符使得字符串向合法的方向更进一步。最后当`left`和`right`均为0的时候，再次检查字符串的合法性（有可能删除的括号并不是真的需要删除的括号）。

## Solution

&emsp;&emsp;题目中包含一个回溯的部分，常规来说回溯应该是从字符串中删掉当前字符，然后调用函数，调用结束后把字符重新添加回去。这里因为是一个字符串，为了代码的简洁，我直接新开一个temp变量去作为函数的参数，在temp中删掉当前字符就可以了，这样就省去了添加回去的步骤。

------

## Code

```c++
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        int left = 0, right = 0;
        for (char ch: s) {
            if (ch == '(') {
                ++left;
            } else if (ch == ')') {
                if (left == 0) {
                    right++;
                } else {
                    left--;
                }
            }
        }
        
        vector<string> result;
        helper(0, s, left, right, result);
        return result;
    }
private:
    bool isValid(string s) {
        int l = 0;
        for (char ch: s) {
            if (ch == '(') {
                l++;
            } else if (ch == ')') {
                l--;
            }
            if (l < 0) {
                return false;
            }
        }
        return l == 0;
    }
    
    void helper(int idx, string s, int left, int right, vector<string>& result) {
        if (left == 0 && right == 0) {
            if (isValid(s)) {
                result.push_back(s);
            }
            return;
        }
        
        for (int i = idx; i < s.size(); ++i) {
            if (i != idx && s[i] == s[i - 1]) {
                continue;
            }
            
            if (s[i] == '(' || s[i] == ')') {
                string temp = s;
                temp.erase(i, 1);
                if (left > 0 && s[i] == '(') {
                    helper(i, temp, left - 1, right, result);
                }
                if (right > 0 && s[i] == ')') {
                    helper(i, temp, left, right - 1, result);
                }
            }
            
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目整体上是一个回溯的思路，并结合了括号合法性判断。通常来说，只要是同一种括号的合法性判断，都可以用计数器的形式去解决问题。这道题目的分享到这里，感谢你的支持！

