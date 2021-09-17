---
title: LeetCode解题报告（407) -- 224. Basic Calculator
mathjax: true
abbrlink: 58236
date: 2021-09-11 18:07:37
---

## Problem

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.

**Note:** You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

<!-- more -->

**Example 1:**

```
Input: s = "1 + 1"
Output: 2
```

**Example 2:**

```
Input: s = " 2-1 + 2 "
Output: 3
```

**Example 3:**

```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
- `s` represents a valid expression.
- `'+'` is not used as a unary operation.
- `'-'` could be used as a unary operation but it has to be inside parentheses.
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.

------

## Analysis

&emsp;&emsp;这道题目内容很简单，计算一个算术表达式。里面只包含`+`、`-`、`(`和`)`，因为没有乘除，所以不用考虑运算符优先级的问题。计算算术表达式的思路还是用栈，栈会使用在有优先级的地方，这里就是括号，因为括号的优先级更高，所以在遇到`(`时，需要先计算括号里面的内容，因此需要把前面计算的结果放入到栈中暂时存储。遇到`)`再把栈中的结果拿出来一起计算。

&emsp;&emsp;处理符号方面，因为只有`+`、`-`，所以我们可以把所有数都看作是加法，减去一个数就看成加上一个负数，因此我们要用额外的标志位记录`sign`。

&emsp;&emsp;处理数字方面没有太多特别的要求，不断循环读取一个完整的数字即可。

&emsp;&emsp;需要提示这里还有一个小坑，字符串中可能含有空格，所以在写判断条件的时候要特别注意。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int calculate(string s) {
        stack<int> st;
        int size = s.size();
        
        int result = 0;
        int sign = 1;
        for (int i = 0; i < size; i++) {
            char ch = s[i];
            if (ch >= '0') {
                int num = 0;
                while (i < size && s[i] >= '0') {
                    num = num * 10 + (s[i] - '0');
                    i++;
                }
                result += num * sign;
                i--;
            } else if (ch == '+') {
                sign = 1;
            } else if (ch == '-') {
                sign = -1;
            } else if (ch == '(') {
                st.push(result);
                st.push(sign);
                sign = 1;
                result = 0;
            } else if (ch == ')') {
                result *= st.top(); // sign
                st.pop();
                result += st.top();
                st.pop();
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然是hard，但是如果处理过加减乘除的算术表达式，这道题目就是easy，其本质和原理都是一样的。这道题目的分享到这里，感谢你的支持！

