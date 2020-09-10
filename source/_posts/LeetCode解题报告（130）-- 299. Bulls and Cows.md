---
title: LeetCode解题报告（130）-- 299. Bulls and Cows
tags:
  - LeetCode
mathjax: true
date: 2020-09-10 18:01:02

---

## Problem

You are playing the following [Bulls and Cows](https://en.wikipedia.org/wiki/Bulls_and_Cows) game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess, use `A` to indicate the bulls and `B` to indicate the cows. 

Please note that both secret number and friend's guess may contain duplicate digits.

<!-- more -->

**Example 1:**

```
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.
```

**Example 2:**

```
Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
```

**Note:** You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

------

## Analysis

&emsp;&emsp;这道是有关一个经典的游戏Bulls and Cows，实际上是两个字符串`secret`和`guess`的匹配问题（题目默认两个字符串长度相等）。题目定义如果相同位置上的数字都相同的话，`bull`加一；如果`guess`中猜中了`secret`的数字（但是这个数字的位置并不匹配的话），`cow`加一。既然是分开两种情况计算，那我们也可以分开两次循环去完成。

&emsp;&emsp;第一个循环计算bull，这个是非常简单的，只需要比对两个字符串的每个位置即可。

&emsp;&emsp;第二个循环计算cow，因为cow的定义是猜对数字但是猜错位置，所以总体上这里猜对的就是数字的出现次数。以Example1为例，除了8之外，0、1、7都是出现了一次，且位置是错误的。和出现次数有关系的自然想到用map来存储，然后在第一个循环的时候减去位置相匹配的，剩下的就用来计算cow，每次secret中出现了就减少一个。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        map<char, int> m;
        int size = secret.size();
        for (int i = 0; i < size; i++) {
            m[guess[i]]++;
        }
        
        int bull = 0, cow = 0;
        for (int i = 0; i < size; i++) {
            if (secret[i] == guess[i]) {
                bull++;
                m[secret[i]]--;
            }
        }
        for (int i = 0; i < size; i++) {
            if (secret[i] != guess[i] && m.count(secret[i]) && m[secret[i]]) {
                m[secret[i]]--;
                cow++;
            } 
        }
        return to_string(bull) + "A" + to_string(cow) + "B";
    }
};
```

------

## Summary

&emsp;&emsp;原本我是把两种处理逻辑放在一个循环里面做的，但是发现计算cow的时候，位置不匹配但是数字出现过的情况就会耗掉bull情况的数字，所以决定分开两个循环做。这道题的分析到这里，谢谢！
