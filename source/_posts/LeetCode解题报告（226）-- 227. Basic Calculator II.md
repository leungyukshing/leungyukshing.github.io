---
title: LeetCode解题报告（226）-- 227. Basic Calculator II
tags:
  - LeetCode
mathjax: true
abbrlink: 36025
date: 2020-11-25 00:47:38
---

## Problem

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only **non-negative** integers, `+`, `-`, `*`, `/` operators and empty spaces ``. The integer division should truncate toward zero.

<!-- more -->

**Example 1:**

```
Input: "3+2*2"
Output: 7
```

**Example 2:**

```
Input: " 3/2 "
Output: 1
```

**Example 3:**

```
Input: " 3+5 / 2 "
Output: 5
```

**Note:**

- You may assume that the given expression is always valid.
- **Do not** use the `eval` built-in library function.

------

## Analysis

&emsp;&emsp;这道题目是比较经典的算术表达式题目，之前没有很系统地分析过，今天就借这道题目来探讨算术表达式的求解。这道题目的表达式中只含数字和加减乘除运算，还有一些空格。因为没有包含括号，所以难度是大大降低。

&emsp;&emsp;先从宏观上来看这类题目的处理思路。一般会使用栈进行求解，原因是运算过程中存在着优先级不同的情况，在遇到优先级高的时候需要先运算，所以栈这个数据结构是比较适合的。回到这道题目，有几种情况是需要特殊处理的：

1. 空格：这种情况遍历的时候直接skip即可；
2. 减法：为了简化代码逻辑，一般减法我们会当成加法来做，加上相反数即可；
3. 乘除：乘除是优先级较高的运算符，因此需要优先计算，这个也是引入栈的原因。

&emsp;&emsp;接下来我就来分析整个处理过程，整体的处理思路是：先提取符号，再提取数字。加减的数字直接入栈，乘除的数字先和栈顶运算，然后入栈。以Example1为例：

1. 一开始默认前面是`+`（第一个运算符）；
2. 提取第一个数字3，入栈；（栈：`top->3`）
3. 提取第二个运算符`+`；
4. 提取第二个的数字2，入栈；（栈：`top->2->3`）
5. 提取第三个运算符`*`；
6. 提取第三个数字2，因为前面的运算符是乘法，所以要立即运算。取出栈顶元素2，运算`2*2 = 4`，然后pop栈顶，再把乘法运算结果push回栈中；（栈：`top->4->3`）
7. 算术表达式都处理完毕，对栈中的元素累加即得到结果。

------

## Solution

&emsp;&emsp;按照上面的思路进行编程即可，需要特别注意的是，我们的处理是分开字符串和数字的，但是要注意当`i = size - 1`的时候，说明是运算到了最后一个数字，同样要进行运算操作，在判断条件不要漏掉。

------

## Code

```c++
class Solution {
public:
    int calculate(string s) {
        stack<int> st;
        long num = 0;
        char op = '+';
        int size = s.size();
        for (int i = 0; i < size; i++) {
            // digit
            if (s[i] >= '0' && s[i] <= '9') {
                num = num * 10 + s[i] - '0';
            }
            // operator
            if (((s[i] < '0' || s[i] > '9') && s[i] != ' ') || i == size - 1) {
                if (op == '+') {
                    st.push(num);
                } else if (op == '-') {
                    st.push(-num);
                } else if (op == '*') {
                    int temp = st.top() * num;
                    st.pop();
                    st.push(temp);
                } else {
                    int temp = st.top() / num;
                    st.pop();
                    st.push(temp);
                }
                op = s[i];
                num = 0;
            }
        }
        
        long result = 0;
        while (!st.empty()) {
            result += st.top();
            st.pop();
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;从这道题目相信能够很好地理解如何使用栈去求解一条算术表达式，这道题目的难度中等，因为它还没有包含括号的情况。而实际上，就算是添加了括号，处理的思路也是类似的。这道题目的分享到这里，谢谢！
