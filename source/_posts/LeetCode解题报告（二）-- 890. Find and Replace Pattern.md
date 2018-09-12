---
title: LeetCode解题报告（二）-- 890. Find and Replace Pattern
tags:
  - LeetCode
abbrlink: 59627
date: 2018-09-05 20:45:04
---
## Problem
You have a list of `words` and a `pattern`, and you want to know which words in `words` matches the pattern.

A word matches the pattern if there exists a permutation of letters `p` so that after replacing every letter `x` in the pattern with `p(x)`, we get the desired word.

*(Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.)*

Return a list of the words in `words` that match the given pattern.

You may return the answer in any order.

**Example1**:
```
Input: words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
Output: ["mee","aqq"]
Explanation: "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}.
"ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation,
since a and b map to the same letter.
```

**Note**:
  + `1 <= words.length <= 50`
  + `1 <= pattern.length = words[i].length <= 20`
<!-- more -->

## Analysis
&emsp;&emsp;这道题目的知识点是**模式匹配**，难点在于不是匹配一个字符串，而是匹配一种模式。题目中斜体部分给我们提示：两个匹配字符串直接必须是双射的映射关系，即**一一对应**。根据这个思路，很直接的一种方法就是使用两个map来建立两个映射，我们只要检查每一个映射是不是都是一对一的就可以了。

## Solution
&emsp;&emsp;在检查匹配的过程中，实质我们是在维护一一对应的关系，也就是说，如果给出一个key，当前对应的字母和已经存在的对应关系不符合，则说明这两个字符串不匹配；如果我们能够在两个字符串之间建立一一对应的关系，则说明这两个字符串模式匹配。

## Code
```C++
class Solution {
public:
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        vector<string> result;
        int size = pattern.size();
        map<char, char> m1;
        map<char, char> m2;

        map<char, char>::iterator it;
        bool flag = true;
        for (int i = 0; i < words.size(); i++) {
            flag = true;
            string temp = words[i];
            for (int i = 0; i < size; i++) {
                m1[pattern[i]] = ' ';
                m2[temp[i]] = ' ';
            }

            for (int j = 0; j < size; j++) {

                it = m1.find(pattern[j]);
                // Not Found
                if (it == m1.end()) {
                    m1[pattern[j]] = temp[j];
                }
                else {
                    if (it->second == ' ') {
                        it->second = temp[j];
                    }
                    else if (it->second != temp[j]) {
                        flag = false;
                    }
                }

                it = m2.find(temp[j]);
                // Not Found
                if (it == m2.end()) {
                    m2[temp[j]] = pattern[j];
                }
                else {
                    if (it->second == ' ') {
                        it->second = pattern[j];
                    }
                    else if (it->second != pattern[j]) {
                        flag = false;
                    }
                }
            }
            if (flag) {
                result.emplace_back(temp);
            }
        }
        return result;
    }
};
```

## Summary
&emsp;&emsp;以往做的关于匹配的题目大多都是匹配某一个确定的字符串，而这次涉及的模式的匹配，是一个全新的方面。我认为关键还是要找到其中的数学原理，只要抓住一一对应这个映射关系，就很容易解决了。本题的分析就到这里，谢谢您的支持！
