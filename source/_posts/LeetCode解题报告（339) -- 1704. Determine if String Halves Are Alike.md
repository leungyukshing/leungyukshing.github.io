---
title: LeetCode解题报告（339) -- 1704. Determine if String Halves Are Alike
tags:
  - LeetCode
mathjax: true
date: 2021-04-12 18:10:48

---

## Problem

You are given a string `s` of even length. Split this string into two halves of equal lengths, and let `a` be the first half and `b` be the second half.

Two strings are **alike** if they have the same number of vowels (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`, `'A'`, `'E'`, `'I'`, `'O'`, `'U'`). Notice that `s` contains uppercase and lowercase letters.

Return `true` *if* `a` *and* `b` *are **alike***. Otherwise, return `false`.

<!-- more -->

**Example 1:**

```
Input: s = "book"
Output: true
Explanation: a = "bo" and b = "ok". a has 1 vowel and b has 1 vowel. Therefore, they are alike.
```

**Example 2:**

```
Input: s = "textbook"
Output: false
Explanation: a = "text" and b = "book". a has 1 vowel whereas b has 2. Therefore, they are not alike.
Notice that the vowel o is counted twice.
```

**Example 3:**

```
Input: s = "MerryChristmas"
Output: false
```

**Example 4:**

```
Input: s = "AbCdEfGh"
Output: true
```

**Constraints:**

- `2 <= s.length <= 1000`
- `s.length` is even.
- `s` consists of **uppercase and lowercase** letters.

------

## Analysis

&emsp;&emsp;题目给出一个字符串，从中间对半切开，要求我们判断前半部分和后半部分中包含的元音字母个数是否相同。

&emsp;&emsp;因为是前后两半对比，所以遍历一半就可以了，每个字符都判断下是否元音，前后两部分各自统计一下元音出现的次数，最后看看两者是否相等即可。

------

## Solution

&emsp;&emsp;注意大小写的问题，

------

## Code

```c++
class Solution {
public:
    bool halvesAreAlike(string s) {
        int size = s.size();
        int num1 = 0, num2 = 0;
        for (int i = 0; i < size / 2; i++) {
            if (isVowel(s[i])) {
                num1++;
            }
            if (isVowel(s[size - 1 - i])) {
                num2++;
            }
        }
        // cout << num1 << " " << num2 << endl;
        return num1 == num2;
    }
private:
    bool isVowel(char c) {
        if (c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U' ||
           c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
            return true;
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道非常简单的字符串遍历题目，没什么难点。这道题目的分享到这里，感谢你的支持！
