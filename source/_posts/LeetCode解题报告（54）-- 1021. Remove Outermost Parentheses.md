---
title: LeetCode解题报告（54）-- 1021. Remove Outermost Parentheses
tags:
  - LeetCode
abbrlink: 56598
date: 2019-09-19 00:53:03
---

## Problem

A valid parentheses string is either empty `("")`, `"(" + A + ")"`, or `A + B`, where `A` and `B` are valid parentheses strings, and `+` represents string concatenation.  For example, `""`, `"()"`, `"(())()"`, and `"(()(()))"` are all valid parentheses strings.

A valid parentheses string `S` is **primitive** if it is nonempty, and there does not exist a way to split it into `S = A+B`, with `A` and `B` nonempty valid parentheses strings.

Given a valid parentheses string `S`, consider its primitive decomposition: `S = P_1 + P_2 + ... + P_k`, where `P_i`are primitive valid parentheses strings.

Return `S` after removing the outermost parentheses of every primitive string in the primitive decomposition of `S`.

<!-- more -->

**Example 1:**

```
Input: "(()())(())"
Output: "()()()"
Explanation: 
The input string is "(()())(())", with primitive decomposition "(()())" + "(())".
After removing outer parentheses of each part, this is "()()" + "()" = "()()()".
```

**Example 2:**

```
Input: "(()())(())(()(()))"
Output: "()()()()(())"
Explanation: 
The input string is "(()())(())(()(()))", with primitive decomposition "(()())" + "(())" + "(()(()))".
After removing outer parentheses of each part, this is "()()" + "()" + "()(())" = "()()()()(())".
```

**Example 3:**

```
Input: "()()"
Output: ""
Explanation: 
The input string is "()()", with primitive decomposition "()" + "()".
After removing outer parentheses of each part, this is "" + "" = "".
```

**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.

------

## Analysis

&emsp;&emsp;这道题是括号匹配的题目，简单的括号匹配相信大家非常熟悉，这道题在匹配的基础上变了花样。题目定义传统意义上满足括号成对匹配的字符串为**primitive valid parentheses string**（PVPS），题目给出一个**valid parentheses string**，我们要做的是把这个字符串拆成多个PVPS（以S=A+B+C的形式拼接）。然后再把这两个PVPS最外层的括号去掉，再重新拼接在一起作为结果输出。

&emsp;&emsp;理解题目花了一点时间，做法仍然是基于stack，这道题最大的难度在于如何去切分成两个PVPS。根据定义，PVPS不能拆成S=A+B的形式，即PVPS外层只能嵌套关系。我们可以从这个关系入手去切分，因为是完整的嵌套关系，所以从stack匹配的角度来说，如果能够匹配，stack就会为空。这样我们就可以有切分的标准了。切分后我们保存好每一部分，最后再去除最外层的括号，然后拼接即可。

------

## Solution

&emsp;&emsp;使用stack匹配括号这个不需要多讲，每当我们匹配到stack为空的时候，则说明可以切分，将字符串存进vector。最后对这个vector的每一个字符串去除头尾的括号，再拼接在一起返回。

------

## Code

```c++
class Solution {
public:
    string removeOuterParentheses(string S) {
        stack<char> st;
        int size = S.size();
        vector<string> arr;
        
        string temp = "";
        for (int i = 0; i < size; i++) {
            temp += S[i];
            if (S[i] == '(') {
                st.push('(');
            }
            else {
                st.pop();
            }
            if (st.empty()) {
                arr.push_back(temp);
                temp = "";
            }
        }
        string result = "";
        int s = arr.size();
        for (int i = 0; i < s; i++) {
            string str = arr[i];
            str.erase(str.size() - 1, 1);
            str.erase(0, 1);
            result += str;
        }
        
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道括号匹配的创新题，在原有的基础上新增了一些变化，难点是要把握住切分的判断时机。这道题的分享就到这里，欢迎大家分享、评论，谢谢您的支持！
