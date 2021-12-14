---
title: LeetCode解题报告（421）-- 214. Shortest Palindrome
mathjax: true
date: 2021-12-13 22:30:57
---

## Problem

You are given a string `s`. You can convert `s` to a palindrome by adding characters in front of it.

Return *the shortest palindrome you can find by performing this transformation*.

<!-- more -->

**Example 1:**

```
Input: s = "aacecaaa"
Output: "aaacecaaa"
```

**Example 2:**

```
Input: s = "abcd"
Output: "dcbabcd"
```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of lowercase English letters only.

------

## Analysis

&emsp;&emsp;这道题目给出一个字符串`s`，要求通过在字符串前面添加**最短的**字符串，使得整个字符串成为一个回文串。我们先来看几种极端情况。第一，如果`s`本来就是一个回文串，那就不需要在前面添加字符串，答案就是`s`本身；第二，如果`s`里面没有包含任何的回文串，那么直接在前面加上`s`的反转，整个字符串就构成了一个回文串。难点是如果`s`中本身包含回文串，怎么处理？

&emsp;&emsp;上面提到`s`中包含回文串，其实更加严谨的说法应该是**回文前缀**，比如`abbac`中，`abba`就是回文前缀，这个才是有价值的。比如`cabba`，这里面的`abba`并没有价值，原因是，在开头的回文串才能利用（成为答案的中间部分），而在后面回文串无法利用。所以我们要找的是回文前缀，寻找的方法也很简单，`i`指向开头，`j`指向结尾，如果两者相同就往中间移动，否则，只移动`j`。最后找到`[0,i)`是前缀，那么只需要把`i`后面的部分反转再添加到前面即可。

&emsp;&emsp;这里还要处理一种情况，通过上面的方法找到的`[0, i)`这个前缀还不一定是回文的，比如`adcba`，最后会找到`i = 2`，但`ad`并不是回文串。这里只需要继续递归处理即可。

## Solution

&emsp;&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    string shortestPalindrome(string s) {
        int i = 0, j = s.size() - 1;
        for (j = s.size() - 1; j >= 0; j--) {
            if (s[i] == s[j]) {
                i++;
            }
        }
        
        if (i == s.size()) {
            return s;
        }
        
        string temp = s.substr(i);
        reverse(temp.begin(), temp.end());
        return temp + shortestPalindrome(s.substr(0, i)) + s.substr(i);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的重点是找回文前缀，后面拼凑的部分就比较容易了。这道题目还有一个做法是采用KMP，看懂了继续和大家分享。这道题目的分享到这里，感谢你的支持！
