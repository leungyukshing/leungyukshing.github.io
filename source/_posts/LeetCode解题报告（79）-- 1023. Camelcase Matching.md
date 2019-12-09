---
title: LeetCode解题报告（79）-- 1023. Camelcase Matching
tags:
  - LeetCode
date: 2019-12-09 22:58:13
mathjax: true
---

## Problem

A query word matches a given `pattern` if we can insert **lowercase** letters to the pattern word so that it equals the `query`. (We may insert each character at any position, and may insert 0 characters.)

Given a list of `queries`, and a `pattern`, return an `answer` list of booleans, where `answer[i]` is true if and only if `queries[i]` matches the `pattern`.

<!-- more -->

**Example 1:**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FB"
Output: [true,false,true,true,false]
Explanation: 
"FooBar" can be generated like this "F" + "oo" + "B" + "ar".
"FootBall" can be generated like this "F" + "oot" + "B" + "all".
"FrameBuffer" can be generated like this "F" + "rame" + "B" + "uffer".
```

**Example 2:**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBa"
Output: [true,false,true,false,false]
Explanation: 
"FooBar" can be generated like this "Fo" + "o" + "Ba" + "r".
"FootBall" can be generated like this "Fo" + "ot" + "Ba" + "ll".
```

**Example 3:**

```
Input: queries = ["FooBar","FooBarTest","FootBall","FrameBuffer","ForceFeedBack"], pattern = "FoBaT"
Output: [false,true,false,false,false]
Explanation: 
"FooBarTest" can be generated like this "Fo" + "o" + "Ba" + "r" + "T" + "est".
```

**Note:**

1. `1 <= queries.length <= 100`
2. `1 <= queries[i].length <= 100`
3. `1 <= pattern.length <= 100`
4. All strings consists only of lower and upper case English letters.

------

## Analysis

&emsp;&emsp;驼峰命名法的字符串匹配，题目给出一个`pattern`以及一系列的`queries`，如果`query`是在pattern的字母之间添加小写字母，则合法；如果添加的是大写字母，则不合法。

&emsp;&emsp;我们对每个查询词遍历，首先判断是否和`pattern`中的字母匹配。如果匹配则到下一个字母；否则检查大小写，小写的可以忽略，遇到大写则说明匹配错误，可以立即中止匹配过程，返回false。

------

## Solution

&emsp;&emsp;由于需要记录查询词和`pattern`匹配到哪一个字母，所以每个查询词都需要有一个下标`k`来记录。同时需要注意，遍历完后有可能是查询词并没有出现`pattern`中所有的字母，所以还要比对`k`和`pattern`的长度。

------

## Code

```c++
class Solution {
public:
    vector<bool> camelMatch(vector<string>& queries, string pattern) {
        vector<bool> result;
        int size = queries.size();
        for (int i = 0; i < size; i++) {
            string str = queries[i];
            int k = 0; // index in pattern
            bool flag = true;
            for (int j = 0; j < str.size(); j++) {
                char ch = str[j];
                if (ch == pattern[k]) {
                    k++;
                }
                else {
                    if (ch >= 'a' && ch <= 'z') {
                        continue;
                    }
                    else {
                        result.push_back(false);
                        flag = false;
                        break;
                    }
                }
            }
            if (flag) {
                if (k < pattern.size()) {
                    result.push_back(false);
                }
                else {
                    result.push_back(true);
                }
            }
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题字符串匹配类型的题目，题目的描述比较少见，但其实本质还是不变的，仍然是通过遍历与比对解决。在面对字符串匹配问题的时候，我们要善于抓住匹配的点，即什么时候匹配失败。抓好这个点就能够写出判断的条件，题目也就迎刃而解。这道题目就和大家分享到这里，欢迎大家评论、转发，谢谢您的支持！