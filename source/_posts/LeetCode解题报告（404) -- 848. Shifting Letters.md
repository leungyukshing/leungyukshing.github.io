---
title: LeetCode解题报告（404) -- 848. Shifting Letters
mathjax: true
date: 2021-09-08 14:21:05
---

## Problem

You are given a string `s` of lowercase English letters and an integer array `shifts` of the same length.

Call the `shift()` of a letter, the next letter in the alphabet, (wrapping around so that `'z'` becomes `'a'`).

- For example, `shift('a') = 'b'`, `shift('t') = 'u'`, and `shift('z') = 'a'`.

Now for each `shifts[i] = x`, we want to shift the first `i + 1` letters of `s`, `x` times.

Return *the final string after all such shifts to s are applied*.

<!-- more -->

**Example 1:**

```
Input: s = "abc", shifts = [3,5,9]
Output: "rpl"
Explanation: We start with "abc".
After shifting the first 1 letters of s by 3, we have "dbc".
After shifting the first 2 letters of s by 5, we have "igc".
After shifting the first 3 letters of s by 9, we have "rpl", the answer.
```

**Example 2:**

```
Input: s = "aaa", shifts = [1,2,3]
Output: "gfd"
```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.
- `shifts.length == s.length`
- `0 <= shifts[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出`shifts`数组，对于每个`shifts[i]`，对前面的`i + 1`个字符shift`shifts[i]`，每一次shift都是在字母表中往后移一位。暴力的解法是遍历shifts数组，然后对于里面每一个值，都去处理`s`的前`i + 1`个字符。但是这样的话，复杂度会相当高，是$O(n^2)$，会pass不了OJ。

&emsp;&emsp;那么我们来看优化的点在哪？shift26次和shift52次对字符来说是没有区别的，我们不需要每次都去实际操作这个字符，相反，我们可以从头到尾计算出每个位置的字符总共要shift多少次，最后再一次过处理即可。仔细观察，`shift[0]`对`s[0]`起作用，`shift[1]`对`s[0]`和`s[1]`起作用，以此类推，我们就能计算出每个位置一共shift了多少次，最后集中处理即可，这样的复杂度就是$O(n)$

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string shiftingLetters(string s, vector<int>& shifts) {
        string result = s;
        int size = shifts.size();
        for (int i = size - 2; i >= 0; i--) {
            shifts[i] = (shifts[i] + shifts[i + 1]) % 26; 
        }
        for (int i = 0; i < size; i++) {
            result[i] = 'a' + ((result[i] - 'a' + shifts[i]) % 26);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的通过集中计算来优化复杂度的流程题目，在处理这类题目时，主要把握好规律，并不需要严格按照题目的要求每步运算求解。这道题目的分享到这里，感谢你的支持！
