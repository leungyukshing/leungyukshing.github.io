---
title: LeetCode解题报告（74）-- 893. Groups of Special-Equivalent Strings
tags:
  - LeetCode
abbrlink: 24311
date: 2019-10-26 14:14:43
---

## Problem

You are given an array `A` of strings.

Two strings `S` and `T` are *special-equivalent* if after any number of *moves*, S == T.

A *move* consists of choosing two indices `i` and `j` with `i % 2 == j % 2`, and swapping `S[i]` with `S[j]`.

Now, a *group of special-equivalent strings from A* is a non-empty subset S of `A` such that any string not in S is not special-equivalent with any string in S.

Return the number of groups of special-equivalent strings from `A`.

<!-- more -->

**Example 1:**

```
Input: ["a","b","c","a","c","c"]
Output: 3
Explanation: 3 groups ["a","a"], ["b"], ["c","c","c"]
```

**Example 2:**

```
Input: ["aa","bb","ab","ba"]
Output: 4
Explanation: 4 groups ["aa"], ["bb"], ["ab"], ["ba"]
```

**Example 3:**

```
Input: ["abc","acb","bac","bca","cab","cba"]
Output: 3
Explanation: 3 groups ["abc","cba"], ["acb","bca"], ["bac","cab"]
```

**Example 4:**

```
Input: ["abcd","cdab","adcb","cbad"]
Output: 1
Explanation: 1 group ["abcd","cdab","adcb","cbad"]
```

**Note:**

- `1 <= A.length <= 1000`
- `1 <= A[i].length <= 20`
- All `A[i]` have the same length.
- All `A[i]` consist of only lowercase letters.

------

## Analysis

&emsp;&emsp;这道题目与字母有关，题目给出一系列的字符串，要求我们对这些字符串划分等价类，划分的标准是：**如果两个字符串在奇数位置或偶数位置进行相互交换后相同，则它们归于同一个等价类**。最终只需要输出不同等价类的个数即可。

&emsp;&emsp;如果我们跟着题目的要求走，我们很容易陷入思维的困境：如何交换才能判断两个字符串是否等价。同时我们要做比较的还不仅仅只是两个字符串，而是多个，所以这种思考方式是无效的。不妨换个思路，我们能不能把这些字符串换成统一的格式来做比对。这个变换的过程我们要保证符合题目给出的交换要求。

&emsp;&emsp;其实这个变换标准不难满足，题目只允许奇偶性相同的位置进行交换，那么我们可以分别把这些位置上的字符拿出来组成两个字符串，然后**排序**（因为排序能提供统一的格式），最后将这两串加起来。这个结果就是我们想做到的统一的格式。

&emsp;&emsp; 所以我们对所有字符串都做这个处理，然后用一个set结果来做统计。这样一看，题目就变得很简单了。

------

## Solution

&emsp;&emsp;使用sort函数排序，用set来统计方便得出最终结果。

------

## Code

```c++
class Solution {
public:
    int numSpecialEquivGroups(vector<string>& A) {
        int size = A.size();
        set<string> s;
        for (int i = 0; i < size; i++) {
            string even = "";
            string odd = "";
            string temp = A[i];
            for (int j = 0; j < temp.size(); j++) {
                if (j % 2 == 0) {
                    even += temp[j];
                }
                else {
                    odd += temp[j];
                }
            }
            sort(even.begin(), even.end());
            sort(odd.begin(), odd.end());
            string all = even + odd;
            s.insert(all);
        }
        return s.size();
    }
};
```

------

## Summary

 &emsp;&emsp;字符串交换、比较的题目我们碰的很多，每次题目都会有些奇奇怪怪的交换规则。我们可以在理解这些规则的基础上，把字符串转为统一的格式处理，这种思路也是一种快速的解题方法，值得总结。本题的分享到这里，欢迎大家转发、评论。最后，谢谢您的支持！
