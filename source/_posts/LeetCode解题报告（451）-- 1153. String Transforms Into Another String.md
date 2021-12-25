---
title: LeetCode解题报告（451）-- 1153. String Transforms Into Another String
mathjax: true
date: 2021-12-25 17:03:13
---

## Problem

Given two strings `str1` and `str2` of the same length, determine whether you can transform `str1` into `str2` by doing **zero or more** *conversions*.

In one conversion you can convert **all** occurrences of one character in `str1` to **any** other lowercase English character.

Return `true` if and only if you can transform `str1` into `str2`.

<!-- more -->

**Example 1:**

```
Input: str1 = "aabcc", str2 = "ccdee"
Output: true
Explanation: Convert 'c' to 'e' then 'b' to 'd' then 'a' to 'c'. Note that the order of conversions matter.
```

**Example 2:**

```
Input: str1 = "leetcode", str2 = "codeleet"
Output: false
Explanation: There is no way to transform str1 to str2.
```

**Constraints:**

- `1 <= str1.length == str2.length <= 104`
- `str1` and `str2` contain only lowercase English letters.

------

## Analysis

&emsp;&emsp;这道题目给出两个字符串`str1`和`str2`，定义conversion为把`str1`中某个字符全部替换成另一个字符，问是否能通过若干个conversion去使得`str1 = str2`。这里讨论的是映射，映射有多种，我们先来看怎么映射法：

1. `str1`中的字符**一一对应**映射到`str2`中的字符，这个是最简单的，例如`abcd`映射到`wxyz`；
2. `str1`中的字符**一对多**映射到`str2`中的字符，这个是不允许的，例如`abab`映射到`wxyz`，`a`无法同时映射到`w`和`y`；
3. `str1`中的字符串**多对一**映射到`str2`中的字符，这个是允许的，例如`abcd`映射到`xxyy`。

&emsp;&emsp;根据上面的分析，我们可以知道只有第二种映射是非法的，其余的情况都是可以成功转化。这里还需要处理一种情况，就是映射死循环。比如`str1 = abcdefghijklmnopqrstuvwxyz`，`str2 = bcdefghijklmnopqrstuvwxyza` ，这种情况下，虽然字符是一一对应，但是存在着循环，比如`str1`中的`a`映射到`b`，然后`b`又映射到`c`，那么前两个字符就会变成`cc`，而不是我们期望的`bc`。

&emsp;&emsp;因此，为了判断第二种条件，我们用一个map来记录映射关系即可，如果`str1`中的字符有多于两个映射，则return false；为了判断映射死循环，我们用一个set来记录在`str2`中被映射过的字符，如果26的个字符都被映射过，说明就是死循环了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool canConvert(string str1, string str2) {
        if (str1 == str2) {
            return true;
        }
        
        unordered_map<char, char> m;
        unordered_set<char> uniqueCharInStr2;
        
        int size = str1.size();
        for (int i = 0; i < size; ++i) {
            char ch1 = str1[i];
            char ch2 = str2[i];
            
            if (m.count(ch1) && m[ch1] != ch2) {
                return false;
            } else if (m.count(ch1) == 0) {
                m[ch1] = ch2;
                uniqueCharInStr2.insert(ch2);
            }
        }
        return uniqueCharInStr2.size() < 26;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度主要在前期的分析上，要分析出映射的类型以及分析出哪些是非法的，哪些是合法的。处理映射类型的题目一般都会用map。这道题目的分享到这里，感谢你的支持！
