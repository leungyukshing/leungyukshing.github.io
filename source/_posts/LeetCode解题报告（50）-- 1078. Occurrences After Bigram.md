---
title: LeetCode解题报告（50）-- 1078. Occurrences After Bigram
tags:
  - LeetCode
date: 2019-09-17 00:26:45
---

## Problem

Given words `first` and `second`, consider occurrences in some `text` of the form "`first second third`", where `second` comes immediately after `first`, and `third` comes immediately after `second`.

For each such occurrence, add "`third`" to the answer, and return the answer.

<!-- more -->

**Example 1:**

```
Input: text = "alice is a good girl she is a good student", first = "a", second = "good"
Output: ["girl","student"]
```

**Example 2:**

```
Input: text = "we will we will rock you", first = "we", second = "will"
Output: ["we","rock"]
```

**Note:**

1. `1 <= text.length <= 1000`
2. `text consists of space separated words, where each word consists of lowercase English letters.`
3. `1 <= first.length, second.length <= 10`
4. `first` and `second` consist of lowercase English letters.

------

## Analysis

&emsp;&emsp;这道题是和字符串匹配有关的，题目给出两个单词，要求在一个句子中顺序匹配这两个单词，然后输出第三个单词。这道题在算法上实现并没有过多的难度，就是简单的遍历。有价值的地方是单词的句子中单词的匹配，在Java或python中，我们可以很快速地使用`split()`来将句子进行切分，但是C++中我们就需要手动实现一次了。

------

## Solution

&emsp;&emsp;首先实现`split()`的功能，以空格为分隔符在句子中切分出单词，得到一个单词的vector。然后对于题目给出的两个单词`first`和`second`进行顺序匹配，注意是先匹配到`first`再接着匹配`second`。

------

## Code

```c++
class Solution {
public:
    vector<string> findOcurrences(string text, string first, string second) {
        vector<string> result;
        vector<string> arr;
        int size = text.size();
        string temp = "";
        for (int i = 0; i < size; i++) {
            if (text[i] == ' ') {
                arr.push_back(temp);
                temp = "";
                continue;
            }
            temp += text[i];
        }
        arr.push_back(temp);
        
        int s = arr.size();
        for (int i = 0; i < s - 2; i++) {
            if (arr[i] == first && arr[i + 1] == second) {
                result.push_back(arr[i + 2]);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道字符串匹配的经典题目，算法思路并没有很难的地方，大家可以通过这道题手动实现一个`split()`，也可以对vector更加熟悉。分享就到这里，欢迎大家转发、评论， 谢谢你的支持！
