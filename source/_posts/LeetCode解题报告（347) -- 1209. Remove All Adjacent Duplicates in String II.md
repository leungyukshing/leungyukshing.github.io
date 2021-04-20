---
title: LeetCode解题报告（347) -- 1209. Remove All Adjacent Duplicates in String II
tags:
  - LeetCode
mathjax: true
date: 2021-04-21 01:13:25

---

## Problem

You are given a string `s` and an integer `k`, a `k` **duplicate removal** consists of choosing `k` adjacent and equal letters from `s` and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make `k` **duplicate removals** on `s` until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

<!-- more -->

**Example 1:**

```
Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.
```

**Example 2:**

```
Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation: 
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"
```

**Example 3:**

```
Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
```

**Constraints:**

- `1 <= s.length <= 105`
- `2 <= k <= 104`
- `s` only contains lower case English letters.

------

## Analysis

&emsp;&emsp;题目给出了一个字符串，还有一个数字`k`，然后来做消消乐。每连续`k`个相同的字母就可以消去，问消去之后剩下的字符串是什么。这道题目的难点是，消去之后，后面的字符串要向前靠，然后再比较，如果不断进行循环的话，就需要不停地检测连续`k`个字符是否相同，这样的复杂度会非常高。因为这里需要一种更加高效的方法去知道是否有连续`k`个字符。

&emsp;&emsp;如果从前往后扫描，只要有连续`k`个相同的字符就消除，后面的顶替上来，这种场景有点类似栈，靠前的是栈底，靠后的是栈顶。当栈顶堆满了`k`个相同的字符后，就把这个元素出栈，后面的继续进来，每次都检查是否栈顶是否有`k`个相同的字符。

------

## Solution

&emsp;&emsp;上面算法的关键是检测栈顶的`k`个元素，为了方便期间，我们不需要每次都取`k`个元素出来，我们可以在栈中直接存储一个pair，分别是字符和它连续出现的次数，这样就方便很多。

------

## Code

```c++
class Solution {
public:
    string removeDuplicates(string s, int k) {
        stack<pair<char, int>> st;
        int size = s.size();
        for (int i = 0; i < size; i++) {
            if (!st.empty()) {
                auto &topElement = st.top();
                // cout << i << " " << topElement.first << " " << topElement.second << endl;
                if (s[i] == topElement.first) {
                    topElement.second++;
                    if (topElement.second == k) {
                        st.pop();
                    }
                } else {
                    st.push(make_pair(s[i], 1));
                    // cout << i << " " << s[i] << endl;
                }
            } else {
                st.push(make_pair(s[i], 1));
                // cout << i << " " << s[i] << endl;
            }
        }

        string result = "";
        while (!st.empty()) {
            auto topElement = st.top();
            st.pop();
            while (topElement.second--) {
                result += topElement.first;
            }
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难点在于借助栈来解决，所以掌握好各种数据结构的特点是非常重要的。使用栈来求解使得题目变得很简单。这道题目的分享到这里，感谢你的支持！
