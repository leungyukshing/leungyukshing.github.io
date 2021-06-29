---
title: LeetCode解题报告（329)-- 423. Reconstruct Original Digits from English
tags:
  - LeetCode
mathjax: true
abbrlink: 59182
date: 2021-04-04 14:38:11
---

## Problem

Given a string `s` containing an out-of-order English representation of digits `0-9`, return *the digits in **ascending** order*.

<!-- more -->

**Example 1:**

```
Input: s = "owoztneoer"
Output: "012"
```

**Example 2:**

```
Input: s = "fviefuro"
Output: "45"
```

**Constraints:**

- `1 <= s.length <= 105`
- `s[i]` is one of the characters `["e","g","f","i","h","o","n","s","r","u","t","w","v","x","z"]`.
- `s` is **guaranteed** to be valid.

------

## Analysis

&emsp;&emsp;题目给出了一个字符串，里面包含着英文数字的英文表示，当然顺序是混乱的。要求我们把对应的阿拉伯数字翻译出来，并且按照从小到大的顺序返回。找到对应的关系并不困难，但是难的是如何在乱序的字符串中，组合出合法的英文单词。但其实这道题目是非常巧妙的。

&emsp;&emsp;我们先来看从0到9的英文单词，在这10个英文单词中，有一些字母是某个单词特有的。比如`z`只有`zero`才有，`w`只有`two`才有，所以当字符串中出现了这些特殊的字母，我们就可以断定，必定有这个数字。这里的前提条件是题目保证了给出的字符串一定是valid的，所以不会有多余的字符。然后有一些字母只出现在两个单词中，而其中一个单词是上面计算过的数字，那么剩下的那个我们就可以知道了。比如`x`只有`six`才有，而`s`是`six`和`seven`都有，所以当我们通过计算`x`出现的次数，就能知道`six`的个数，然后计算出`s`的个数，减去`six`的个数，结果就是`seven`的个数了。按照这个思路下去，有些字符出现在三个单词中，如果我们知道其中的两个单词的出现次数，那么剩下的那个就能计算出来。于是就有了下面这个表：

- `z`: `zero`
- `w`: `two`
- `u`: `four`
- `x`: `six`
- `g`: `eight`
- `s`: `six`, `seven`
- `h`: `three`, `eight`
- `f`: `four`, `five`
- `o`: `zero`, `one`, `two`, `four`
- `i`: `five`, `six`, `eight`, `nine`

&emsp;&emsp;求出每个数字出现的次数后，最后再一次性转化为字符串即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string originalDigits(string s) {
        int count[26] = {0};
        int size = s.size();
        for (int i = 0; i < size; i++) {
            count[s[i]- 'a']++;
        }
        
        vector<int> num(10, 0);
        num[0] = count['z' - 'a'];
        num[2] = count['w' - 'a'];
        num[4] = count['u' - 'a'];
        num[6] = count['x' - 'a'];
        num[8] = count['g' - 'a'];
        num[1] = count['o' - 'a'] - num[0] - num[2] - num[4];
        num[7] = count['s' - 'a'] - num[6];
        num[3] = count['h' - 'a'] - num[8];
        num[5] = count['f' - 'a'] - num[4];
        num[9] = count['i' - 'a'] - num[5] - num[6] - num[8];
        
        string result = "";
        
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < num[i]; j++) {
                result += to_string(i);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
