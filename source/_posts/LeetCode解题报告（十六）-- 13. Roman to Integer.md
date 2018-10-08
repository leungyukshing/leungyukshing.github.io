---
title: LeetCode解题报告（十六）-- 13. Roman to Integer
tags:
  - LeetCode
abbrlink: 32330
date: 2018-10-08 14:20:40
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

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.
<!-- more -->

**Example 1**:
```
Input: "III"
Output: 3
```

**Example 2**:
```
Input: "IV"
Output: 4
```

**Example 3**:
```
Input: "IX"
Output: 9
```

**Example 4**:
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5**:
```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Analysis
&emsp;&emsp;这题考察的是字符与数字之间的转换，与以往的题目不同，这次是要从**罗马数字**转为**阿拉伯数字**。罗马数字用的是字母来表示的，且他的表示方式是固定的，因此，我们可以通过建立一张转换表来进行换算。难点在于，部分字母连在一起所表达的意思是不同的。如`IV`表示的是4，`XL`表示的是40。这就需要我们特殊处理。

## Solution
&emsp;&emsp;建立字符与数字的对应关系，很自然地想到了用map来实现。然后在遍历时特殊处理六种情况就可以了。

## Code
```C++
/*
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
*/

class Solution {
public:
    int romanToInt(string s) {
        m['I'] = 1;
        m['V'] = 5;
        m['X'] = 10;
        m['L'] = 50;
        m['C'] = 100;
        m['D'] = 500;
        m['M'] = 1000;


        int result = 0;
        for (int i = 0; i < s.size(); i++) {
            if (i != s.size() - 1) {
                if (s[i] == 'I' && s[i+1] == 'V') {
                    result += 4;
                    i++;
                }
                else if (s[i] == 'I' && s[i+1] == 'X') {
                    result += 9;
                    i++;
                }
                else if (s[i] == 'X' && s[i+1] == 'L') {
                    result += 40;
                    i++;
                }
                else if (s[i] == 'X' && s[i+1] == 'C') {
                    result += 90;
                    i++;
                }
                else if (s[i] == 'C' && s[i+1] == 'D') {
                    result += 400;
                    i++;
                }
                else if (s[i] == 'C' && s[i+1] == 'M') {
                    result += 900;
                    i++;
                }
                else {
                    result += m[s[i]];
                }
            }
            else {
                result += m[s[i]];
            }
        }
        return result;
    }
private:
    map<char, int> m;
};
```
**运行时间：**约48ms，超过90.55%的CPP代码。
## Summary
&emsp;&emsp;这道题目虽然很新，但是原理和字符数字转换的题目是一样的。只不过以往我们是基于ASCII的编码来进行转换，而这里需要我们人工定义转换的对应关系。弄懂了这个，整道题也就不难了。本题的题析到这里，谢谢您的支持！
