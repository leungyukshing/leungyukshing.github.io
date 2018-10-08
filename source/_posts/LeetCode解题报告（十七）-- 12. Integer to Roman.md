---
title: LeetCode解题报告（十七）-- 12. Integer to Roman
tags:
  - LeetCode
abbrlink: 59881
date: 2018-10-08 14:46:10
---
## Problem
Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:
  + `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
  + `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
  + `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.
<!-- more -->

**Example 1**:
```
Input: 3
Output: "III"
```

**Example 2**:
```
Input: 4
Output: "IV"
```

**Example 3**:
```
Input: 9
Output: "IX"
```

**Example 4**:
```
Input: 58
Output: "LVIII"
Explanation: C = 100, L = 50, XXX = 30 and III = 3.
```

**Example 5**:
```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Analysis
&esmp;&emsp;这道题与[Roman to Integer](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%8D%81%E5%85%AD%EF%BC%89--%2013.%20Roman%20to%20Integer.html)恰好是相反的思路。由阿拉伯数字转变成罗马数字，关键在于要从高位开始算，因此我们只需要知道有多少种符号就可以了。根据题目要求，我们可以知道共有13种符号，也就是说，我们共有13种不同的**位**。

## Solution
&emsp;&emsp;与前面一题相似，我们需要存储两种数字之间的对应关系，但是因为这里需要同时对应符号和**位**两个集合，而又要进行循环，因此使用map的话会比较麻烦，而使用数组的话可以用下标直接访问，快速很多。我们需要找出每一**位**出现的次数，然后添加对应的符号即可。注意需要将特殊情况的符号和**位**考虑在内。

## Code
```C++
class Solution {
public:
    string intToRoman(int num) {
        string result = "";
        string roman[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        int values[] = {1000, 900, 500, 400, 100, 90, 50, 40 ,10, 9, 5, 4, 1};

        for (int i = 0; i < 13; i++) {
            while (num >= values[i]) {
                num -= values[i];
                result += roman[i];
            }
        }
        return result;
    }
};
```
**运行时间：**约44ms，超过86.92%的CPP代码。

## Summary
&emsp;&emsp;综合两题关于数字之间的转换，其核心思想还是建立两种进制的对应关系。而使用map还是数组，是要看哪个方便，一般来说使用map会更加方便，但是考虑到这题中会有两个字符的符号，所以使用数组会更加方便。本题的解析到这里，谢谢您的支持！
