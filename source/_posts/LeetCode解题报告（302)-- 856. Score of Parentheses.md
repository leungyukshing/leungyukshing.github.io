---
title: LeetCode解题报告（302)-- 856. Score of Parentheses
tags:
  - LeetCode
mathjax: true
date: 2021-02-27 15:08:14

---

## Problem

Given a balanced parentheses string `S`, compute the score of the string based on the following rule:

- `()` has score 1
- `AB` has score `A + B`, where A and B are balanced parentheses strings.
- `(A)` has score `2 * A`, where A is a balanced parentheses string.

<!-- more -->

**Example 1:**

```
Input: "()"
Output: 1
```

**Example 2:**

```
Input: "(())"
Output: 2
```

**Example 3:**

```
Input: "()()"
Output: 2
```

**Example 4:**

```
Input: "(()(()))"
Output: 6
```

**Note:**

1. `S` is a balanced parentheses string, containing only `(` and `)`.
2. `2 <= S.length <= 50`

------

## Analysis

&emsp;&emsp;这道题目要求对给定的括号字符串进行计分，括号成对算分，规则如下：

- 单对括号：计1分；
- 并排括号：分数相加；
- 包含括号：分数乘2。

&emsp;&emsp;因为要处理包含的部分，所以采用递归来做。我们找出最外层匹配的括号，然后对里面的内容进行递归，求出分数后，再乘2。并列关系的直接相加即可。这里的问题就转化为：如何找出最外层匹配的括号，然后对里面的内容进行递归。

&emsp;&emsp;因为题目给出的括号类型只有一种，所以我们可以用简单的计数器的形式去找到匹配的括号，遇到`(`自增，遇到`)`自减，如果计数器重置为0，说明找到一对匹配的括号，然后对其中的内容进行递归即可。

------

## Solution

&emsp;&emsp;有一个注意点，如果括号里面是空，那么递归的结果会是0，此时因为递归结束后要乘2，如果是0的话这个分数就不对了，所以要用一个`max(2 * cur, 1)`。

------

## Code

```c++
class Solution {
public:
    int scoreOfParentheses(string S) {
        int res = 0, n = S.size();
    	for (int i = 0; i < n; ++i) {
    		if (S[i] == ')') continue;
			int pos = i + 1, cnt = 1;
			while (cnt != 0) {
    			(S[pos++] == '(') ? ++cnt : --cnt;
    		}
    		int cur = scoreOfParentheses(S.substr(i + 1, pos - i - 2));
    		res += max(2 * cur, 1);
    		i = pos - 1;
    	}
    	return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是括号题目的一个变种题，其重点不再是括号的匹配，而是对匹配的括号计算分数，但也利用到了使用计数器进行匹配的技巧。同时，对于字符串中有包含关系的逻辑，可以优先考虑用递归解决。这道题目的分享到这里，谢谢您的支持！
