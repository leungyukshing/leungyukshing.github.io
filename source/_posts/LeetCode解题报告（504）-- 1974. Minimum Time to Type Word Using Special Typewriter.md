---
title: LeetCode解题报告（504）-- 1974. Minimum Time to Type Word Using Special Typewriter
mathjax: true
date: 2022-01-10 22:02:23
---

## Problem

There is a special typewriter with lowercase English letters `'a'` to `'z'` arranged in a **circle** with a **pointer**. A character can **only** be typed if the pointer is pointing to that character. The pointer is **initially** pointing to the character `'a'`.

<!-- more -->

![Chart](https://assets.leetcode.com/uploads/2021/07/31/chart.jpg)

Each second, you may perform one of the following operations:

- Move the pointer one character **counterclockwise** or **clockwise**.
- Type the character the pointer is **currently** on.

Given a string `word`, return the **minimum** number of seconds to type out the characters in `word`.

 

**Example 1:**

```
Input: word = "abc"
Output: 5
Explanation: 
The characters are printed as follows:
- Type the character 'a' in 1 second since the pointer is initially on 'a'.
- Move the pointer clockwise to 'b' in 1 second.
- Type the character 'b' in 1 second.
- Move the pointer clockwise to 'c' in 1 second.
- Type the character 'c' in 1 second.
```

**Example 2:**

```
Input: word = "bza"
Output: 7
Explanation:
The characters are printed as follows:
- Move the pointer clockwise to 'b' in 1 second.
- Type the character 'b' in 1 second.
- Move the pointer counterclockwise to 'z' in 2 seconds.
- Type the character 'z' in 1 second.
- Move the pointer clockwise to 'a' in 1 second.
- Type the character 'a' in 1 second.
```

**Example 3:**

```
Input: word = "zjpc"
Output: 34
Explanation:
The characters are printed as follows:
- Move the pointer counterclockwise to 'z' in 1 second.
- Type the character 'z' in 1 second.
- Move the pointer clockwise to 'j' in 10 seconds.
- Type the character 'j' in 1 second.
- Move the pointer clockwise to 'p' in 6 seconds.
- Type the character 'p' in 1 second.
- Move the pointer counterclockwise to 'c' in 13 seconds.
- Type the character 'c' in 1 second.
```

**Constraints:**

- `1 <= word.length <= 100`
- `word` consists of lowercase English letters.

---

## Analysis

&emsp;&emsp;这道题目给出了一个圆盘的打字机，给了一个字符串`word`，起始指针指向`a`，每打一个字符的cost是移动的距离，问打完这个字符串最小的cost是多少。这道题目实际上就是问从某个字符到另一个字符，最短的路径是什么。无非就两种选择，顺时针和逆时针。顺时针就是`end - start`，逆时针就是`start - end`，为了统一处理都加上26变成正数，然后对26取余即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minTimeToType(string word) {
        int result = 0;
        char pre = 'a';
        for (char ch: word) {
            result += min((ch - pre + 26) % 26, (pre - ch + 26) % 26);
            pre = ch;
        }
        return result + word.size();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质就是考察在循环数组中如何处理下标问题。这道题目的分享到这里，感谢你的支持！

