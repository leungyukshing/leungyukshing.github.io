---
title: LeetCode解题报告（270）-- 1641. Count Sorted Vowel Strings
tags:
  - LeetCode
mathjax: true
date: 2021-01-17 20:16:24


---

## Problem

Given an integer `n`, return *the number of strings of length* `n` *that consist only of vowels (*`a`*,* `e`*,* `i`*,* `o`*,* `u`*) and are **lexicographically sorted**.*

A string `s` is **lexicographically sorted** if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

<!-- more -->

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: The 5 sorted strings that consist of vowels only are ["a","e","i","o","u"].
```

**Example 2:**

```
Input: n = 2
Output: 15
Explanation: The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.
```

**Example 3:**

```
Input: n = 33
Output: 66045
```

**Constraints:**

- `1 <= n <= 50` 

------

## Analysis

&emsp;&emsp;题目给出一个数字`n`，代表字符串的长度，规定字符串只能由元音字母构成，而且必须是字典序，问一共有多少种组合。这道题目有暴力求解，也能够使用动态规划，但这里我分享一个利用数学推理来解决的方法。

&emsp;&emsp;对于最终的字符串，我们假假设选取了五个原因字符分别有a、e、i、o、u个。实际上我们可以想象成，有`n`个小球，我们需要给这些小球标上这些元音字符（5种且按照字典序），问有多少种分割的方法。为了分出5种类型，我们需要插入4块隔板，插入的位置有`n - 1`个。为什么是`n - 1`呢？因为这样保证了每种类型都至少有一个，在这个情况下，总数是$C^{m-1}_{n-1}$那么问题来了，题目的条件是可以有部分元音不出现，所以我们希望的是隔板之间可以为空的情况。

&emsp;&emsp;我们如何解决这个问题呢？其实很简单，我们只需要多要`m`个小球，首先分到`m`个盒子，然后再按照上面的情况处理，最后再把这`m`个小球都拿走，得到的结果就是`n`个球分到`m`个盒子，盒子可以为空的情况了，所以答案就是$C_{n+m-1}^{m - 1}$。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int countVowelStrings(int n) {
        return ((n + 4) * (n + 3) * (n + 2)  * (n + 1)) / 24;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目从数学的角度求解，思考过程比较复杂，也比较考察排列组合方面的知识，但是coding量就非常少了。这道题这道题目的分享到这里，谢谢您的支持！
