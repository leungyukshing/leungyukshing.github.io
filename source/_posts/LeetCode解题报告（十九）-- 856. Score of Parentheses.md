---
title: LeetCode解题报告（十九）-- 856. Score of Parentheses
tags:
  - LeetCode
abbrlink: 36629
date: 2018-10-16 00:15:16
---
## Problem
Given a balanced parentheses string `S`, compute the score of the string based on the following rule:
  + `()` has score 1
  + `AB` has score `A + B`, where A and B are balanced parentheses strings.
  + `(A)` has score `2 * A`, where A is a balanced parentheses string.
<!-- more -->

**Example 1**:
```
Input: "()"
Output: 1
```

**Example 2**:
```
Input: "(())"
Output: 2
```

**Example 3**:
```
Input: "()()"
Output: 2
```

**Example 4**:
```
Input: "(()(()))"
Output: 6
```

**Note:**
  1. `S` is a balanced parentheses string, containing only `(` and `)`.
  2. `2 <= S.length <= 50`

## Analysis
&emsp;&emsp;这是一个关于括号算分的题目，与匹配没有太大关系，因此不需要用到栈。计分的规则主要看深度`depth`。对几个例子分析后可以得出，我们只有在碰到`)`的时候才会进行计分（且前一个是'('，即成对出现时才计分）。得分是当前得分**右移**嵌套深度`depth`。

## Solution
&emsp;&emsp;使用一个变量`depth`来记录嵌套深度。另外使用一个变量`last_ch`记录上一个字符，以确定遇到`)`后是否计分。

## Code
```C++
class Solution {
public:
    int scoreOfParentheses(string S) {
        int depth = 0;
        int score = 0;
        char last_ch;
        for (int i = 0; i < S.size(); i++) {
            if (S[i] == '(') {
                depth++;
            }
            else {
                depth--;
                if (last_ch == '(') {
                    score += 1 << depth;
                }

            }
            last_ch = S[i];
        }
        return score;
    }
};
```
**运行时间：**约0ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;这道题第一眼看下去有点复杂，牵涉到了嵌套计分的问题，但是发现规律后就很容易解决了。抓住只有遇到成对的`()`的才算分这个特点，其余的都是用来计算嵌套深度的，所以整个计算也就很简单了。这道题的分析到这里了，谢谢您的支持！
