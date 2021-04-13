---
title: LeetCode解题报告（340) -- 17. Letter Combinations of a Phone Number
tags:
  - LeetCode
mathjax: true
date: 2021-04-13 01:41:12

---

## Problem

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<!-- more -->

![Example](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

------

## Analysis

&emsp;&emsp;题目给出一串数字，每个数字和字母的对应关系参考电话上的位置，要求把所有的组合都返回。因为每增加一个数字，都需要把改数字映射的字母都加上去，所以这个是很典型的BFS了。我们使用层数来判断是否终止，因为字符串的长度比定是等于数字的长度，所以迭代中带上层数作为参数；同时因为要组成字符串，所以当前拼接的字符串也是一个参数；然后还需要一个参数传入引用，用于存放结果。

&emsp;&emsp;每次迭代时，首先判断是否满足终止条件。如果满足的话就把当前拼接的字符串放入到结果中；反之，则找到该数字对应的字符串，然后逐个字母添加进去，重新迭代。

------

## Solution

&emsp;&emsp;为了方便期间，可以把数字和字母的映射写成一个数组，方便迭代中访问。

------

## Code

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) {
            return {};
        }
        string dict[] = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        vector<string> result;
        int size = digits.size();
        
        helper(digits, dict, 0, "", result);
        return result;
    }
private:
    void helper(string digits, string dict[], int level, string out, vector<string>& result) {
        if (level == digits.size()) {
            result.push_back(out);
            return;
        }
        string str = dict[digits[level] - '2'];
        for (int i = 0; i < str.size(); i++) {
            helper(digits, dict, level + 1, out + str[i], result);
        }
    }
};
```

------

## Summary

&emsp;&emsp;这是一道字符串类型题目中很经典的BFS递归，这类题目的特点是需要找出字符串本身，所以在递归的参数中需要带上，然后要把终止条件带上，这里用了层数。这道题目的分享到这里，感谢你的支持！
