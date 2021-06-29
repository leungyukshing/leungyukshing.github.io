---
title: LeetCode解题报告（70）-- 1002. Find Common Characters
tags:
  - LeetCode
abbrlink: 26466
date: 2019-10-11 17:59:12
---

## Problem

Given an array `A` of strings made only from lowercase letters, return a list of all characters that show up in all strings within the list **(including duplicates)**.  For example, if a character occurs 3 times in all strings but not 4 times, you need to include that character three times in the final answer.

You may return the answer in any order.

<!-- more -->

**Example 1:**

```
Input: ["bella","label","roller"]
Output: ["e","l","l"]
```

**Example 2:**

```
Input: ["cool","lock","cook"]
Output: ["c","o"]
```

**Note:**

1. `1 <= A.length <= 100`
2. `1 <= A[i].length <= 100`
3. `A[i][j]` is a lowercase letter

------

## Analysis

&emsp;&emsp;这道题目给出一系列单词，要求我们统计在每个单词都出现过的字符。遇到统计问题我们都会很自然地使用map来解决，所以统计每个单词中字符的问题迎刃而解。难点在于如果统计总的字符。实际上这是一个取**极小值**问题，因为如果三个单词A、B、C中，某字符在A出现了3次，在B出现了1次，那么总共common的次数就是1次。因此我们还需要一个总的map来存放最终的结果。

------

## Solution

&emsp;&emsp;使用两个map分别做单个单词和整道题目的统计，需要注意的是，在取极小值的时候，第一个map不需要参与，因为一开始总map的value都是0，需要第一个map来打破这个0。

------

## Code

```c++
class Solution {
public:
    vector<string> commonChars(vector<string>& A) {
        map<char, int> m;
        int size = A.size();
        for (int i = 0; i < size; i++) {
            string str = A[i];
            int s = str.size();
            map<char, int> temp;
            for (int j = 0; j < s; j++) {
                temp[str[j]]++;
            }
            for (int j = 0; j < 26; j++) {
                if (i == 0) {
                    m[j + 'a'] = temp[j + 'a'];
                }
                else {
                    m[j + 'a'] = min(m[j + 'a'], temp[j + 'a']);
                }
            }
        }
        vector<string> result;
        for (map<char, int>::iterator iter = m.begin(); iter != m.end(); iter++) {
            if (iter->second == 0) {
                continue;
            }
            string s(1, iter->first);
            result.insert(result.end(), iter->second, s);
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目用到了两个map来解决，基本原理都是统计问题，主要是注意一开始的取最小值的情况就可以了。这道题的分享就到这里，欢迎大家评论、转发，谢谢你的支持！