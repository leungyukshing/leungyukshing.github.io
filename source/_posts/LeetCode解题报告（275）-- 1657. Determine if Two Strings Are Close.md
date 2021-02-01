---
title: LeetCode解题报告（275）-- 1657. Determine if Two Strings Are Close
tags:
  - LeetCode
mathjax: true
date: 2021-02-02 01:19:48

---

## Problem

Two strings are considered **close** if you can attain one from the other using the following operations:

- Operation 1: Swap any two **existing** characters.
  - For example, `abcde -> aecdb`
- Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
  - For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` *if* `word1` *and* `word2` *are **close**, and* `false` *otherwise.*

<!-- more -->

**Example 1:**

```
Input: word1 = "abc", word2 = "bca"
Output: true
Explanation: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"
```

**Example 2:**

```
Input: word1 = "a", word2 = "aa"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any number of operations.
```

**Example 3:**

```
Input: word1 = "cabbba", word2 = "abbccc"
Output: true
Explanation: You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
Apply Operation 2: "caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"
```

**Example 4:**

```
Input: word1 = "cabbba", word2 = "aabbss"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any amount of operations.
```

**Constraints:**

- `1 <= word1.length, word2.length <= 105`
- `word1` and `word2` contain only lowercase English letters.

------

## Analysis

&emsp;&emsp;题目给出两个字符串，同时给出两种操作的方法，问通过这两种操作能否把这两个字符串变为一样。操作有两种：

1. 任意交换两个字符；
2. 把一种字符全部替换为另一种字符。

&emsp;&emsp;真的去每个字符都试一下这两种操作复杂度非常高，我们直接来看看这两个操作能不能简化成一些条件判断。

1. 字符串长度相等。两个字符串要通过操作变成一样，首先要保证长度相等；
2. 字符串里面的字母种类是一样的。无论怎么变化，不可能有新的字符引进，也不能删去字符，所以两个字符串里面的字母种类是一样的；
3. 字母出现次数的是一样的。注意这里说的是字母出现的次数，而不是每个字母出现的次数都必须是一样的。这个对应操作2，因为只要出现的次数是一样的，就能够应用操作2。例如`caabbb`中，`a`、`b`、`c`三个字母出现的次数分别是2、3、1，而在`baaccc`中，出现的次数分别是2、1、3。在这种情况下，可以通过把所有的`b`换成`c`，把所有的`c`换成`b`即可。

------

## Solution

&emsp;&emsp;第一个条件判断非常容易。第二个条件判断可以使用set，这里简单起见我直接使用数组。第三个条件有点技巧，因为我们只关心出现的次数，而并不关心是哪个字符，所以统计完后直接排序，如果排序后两个结果相等，说明满足这个条件。

------

## Code

```c++
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        int size1 = word1.size(), size2 = word2.size();
        
        if (size1 != size2) {
            return false;
        }
        
        vector<int> keys1(26, 0), keys2(26, 0);
        vector<int> count1(26, 0), count2(26, 0);
        
        for (char c: word1) {
            keys1[c - 'a'] = 1;
            count1[c - 'a']++;
        }
        for (char c: word2) {
            keys2[c - 'a'] = 1;
            count2[c - 'a']++;
        }
        sort(count1.begin(), count1.end());
        sort(count2.begin(), count2.end());
        return keys1 == keys2 && count1 == count2;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是字符串题目中挺有意思的创新题，这类题目虽然在算法上没有很特别的地方，但是很考察对规律的把握。找准判断的条件就能够很容易地解决这种题目，所以还是需要多多积累。这道题这道题目的分享到这里，谢谢您的支持！
