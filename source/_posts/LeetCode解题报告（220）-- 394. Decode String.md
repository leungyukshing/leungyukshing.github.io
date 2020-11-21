---
title: LeetCode解题报告（220）-- 394. Decode String
tags:
  - LeetCode
mathjax: true
date: 2020-11-20 21:43:11

---

## Problem

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times. Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*. For example, there won't be input like `3a` or `2[4]`.

<!-- more -->

**Example 1:**

```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

**Example 2:**

```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

**Example 3:**

```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

**Exampl 4:**

```
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```

**Constraints:**

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
- `s` is guaranteed to be **a valid** input.
- All the integers in `s` are in the range `[1, 300]`.

------

## Analysis

&emsp;&emsp;这道题是字符串解码的题目。题目使用了特定的方式压缩字符串，既把字符串重复出现的次数放在前面，然后把字符串放在`[]`内。要求我们根据这个规则对字符串重新解码。因为是可以嵌套的，所以毫无疑问这里优先考虑使用递归。

&emsp;&emsp;在遍历字符串的时候，我们会遇到如下几种情况：

1. 正常的字母：遇到字母说明是字符串的部分，应该用一个变量存起来，为了后续递归（因为有可能重复多次）；
2. 数字字符：如果遇到了数字字符，则需要一直往后遍历直到不是数字，把字符串转为数字，这个就是后面的字符串出现的次数；
3. `[`：遍历到这个符号说明递归开始，因此需要用一个变量把后面遍历到的字符串都存起来。
4. `]`：遍历到这个符号说明字符串结束，可以调用递归。

------

## Solution

&emsp;&emsp;按照上面的分析coding即可，需要注意当遍历到`[`或`]`的时候千万不要加入到字符串中，下标要自增跳过。

------

## Code

```c++
class Solution {
public:
    string decodeString(string s) {
        int i = 0;
        return helper(s, i);
    }
private:
    string helper(string s, int& i) {
        string result = "";
        int size = s.size();
        while (i < size && s[i] != ']') {
            if (s[i] < '0' || s[i] > '9') {
                // alphabet
                result += s[i];
                i++;
            } else {
                int times = 0;
                while (s[i] >= '0' && s[i] <= '9') {
                    times = times * 10 + (s[i] - '0');
                    i++;
                }
                i++;
                string temp = helper(s, i);
                i++;
                while (times) {
                    result += temp;
                    times--;
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是字符串里面有一定难度的题目，要通过递归来进行求解。而实际上当我们分析出几个关键的字符变化点后，是比较容易处理的。这道题目的分享到这里，谢谢！
