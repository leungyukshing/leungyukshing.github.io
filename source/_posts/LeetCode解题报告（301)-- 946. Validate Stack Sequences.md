---
title: LeetCode解题报告（301)-- 946. Validate Stack Sequences
tags:
  - LeetCode
mathjax: true
abbrlink: 13098
date: 2021-02-27 13:05:41
---

## Problem

Given two sequences `pushed` and `popped` **with distinct values**, return `true` if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

<!-- more -->

**Example 1:**

```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**Example 2:**

```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.
```

**Constraints:**

- `0 <= pushed.length == popped.length <= 1000`
- `0 <= pushed[i], popped[i] < 1000`
- `pushed` is a permutation of `popped`.
- `pushed` and `popped` have distinct values.

------

## Analysis

&emsp;&emsp;这是一道和stack相关的题目。题目给出两个数组`pushed`和`popped`，要求我们校验能否从`pushed`数组，通过一个stack，产生出`popped`数组。思路也很清晰，直接用一个stack模拟即可。把`pushed`中的元素逐个放入到stack中，每次放入后，和`popped`中的做对比，如果栈顶元素和`popped`中的相同，元素出栈，`popped`的下标自增，如果到最后栈为空并且`popped`的下标到了末尾，说明所有的元素都能处理完，return true；否则return false。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st;
        int size = pushed.size();
        int idx1 = 0, idx2 = 0;
        while (idx1 != size) {
            st.push(pushed[idx1++]);
            while (!st.empty() && st.top() == popped[idx2]) {
                st.pop();
                idx2++;
            }
        }
        return st.empty() && idx2 == size;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是栈的利用，它从另一个角度考察了我们对栈的理解。这道题目的分享到这里，谢谢您的支持！
