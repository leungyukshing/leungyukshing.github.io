---
title: LeetCode解题报告（382) -- 150. Evaluate Reverse Polish Notation
tags:
  - LeetCode
mathjax: true
abbrlink: 39328
date: 2021-06-06 10:47:11
---

## Problem

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.

<!-- more -->

**Example 1:**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```



**Constraints:**

- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

------

## Analysis

&emsp;&emsp;这道题目是逆波兰表达式的运算。题目给出的顺序已经是逆波兰表达式的形式了，而不需要我们从算术运算式转为逆波兰，所以这道题目实际上是非常简单了。逆波兰表达式的计算只需要用一个栈实现，遇到数字入栈，遇到运算符后，取栈顶的两个元素出来运算，然后再放回去，最后剩下的那个数就是结果了。

&emsp;&emsp;有个细节需要注意，取栈顶两个元素出来运算时，最上面的是第二个操作数，第二上面的才是第一个操作数，注意这里的顺序不要弄错。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> s;
        for (auto token: tokens) {
            if (token == "+" || token == "-" || token == "*" || token == "/") {
                int op1 = s.top();
                s.pop();
                int op2 = s.top();
                s.pop();
                
                int temp = 0;
                if (token == "+") {
                    temp = op2 + op1;
                } else if (token == "-") {
                    temp = op2 - op1;
                } else if (token == "*") {
                    temp = op2 * op1;
                } else {
                    temp = op2 / op1;
                }
                s.push(temp);
            } else {
                s.push(stoi(token));
            }
        }
        return s.top();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
