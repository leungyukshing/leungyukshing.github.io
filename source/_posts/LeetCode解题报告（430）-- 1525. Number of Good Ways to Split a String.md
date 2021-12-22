---
title: LeetCode解题报告（430）-- 1525. Number of Good Ways to Split a String
mathjax: true
date: 2021-12-22 00:02:42
---

## Problem

You are given a string `s`, a split is called *good* if you can split `s` into 2 non-empty strings `p` and `q` where its concatenation is equal to `s` and the number of distinct letters in `p` and `q` are the same.

Return the number of *good* splits you can make in `s`.

<!-- more -->

**Example 1:**

```
Input: s = "aacaba"
Output: 2
Explanation: There are 5 ways to split "aacaba" and 2 of them are good. 
("a", "acaba") Left string and right string contains 1 and 3 different letters respectively.
("aa", "caba") Left string and right string contains 1 and 3 different letters respectively.
("aac", "aba") Left string and right string contains 2 and 2 different letters respectively (good split).
("aaca", "ba") Left string and right string contains 2 and 2 different letters respectively (good split).
("aacab", "a") Left string and right string contains 3 and 1 different letters respectively.
```

**Example 2:**

```
Input: s = "abcd"
Output: 1
Explanation: Split the string as follows ("ab", "cd").
```

**Example 3:**

```
Input: s = "aaaaa"
Output: 4
Explanation: All possible splits are good.
```

**Example 4:**

```
Input: s = "acbadbaada"
Output: 2
```

**Constraints:**

- `s` contains only lowercase English letters.
- `1 <= s.length <= 10^5`

------

## Analysis

&emsp;&emsp;题目给出一个字符串，要求对字符串做一个切分，切分成左右两部分，如果两部分中unique的字符数量一样，则这是一个good split，问一共有多少个good split。这道题目很直观的做法就是，每个位置都作为切分点，然后分别统计左右两边的unique字符数量，如果一样结果加一。但这种做法会TLE，原因是，每次都需要计算两部分的unique字符数量。

&emsp;&emsp;知道为什么超时，那我们就从这个问题入手。计算unique字符这个操作如果提取出来，预先计算好呢？说到底我们要的是左边的unique字符数量以及右边的unique字符数量，而这个信息是不会随着切分点的移动而改变的，我们可以预先计算好，类似一个presum的思路。从左右两边同时遍历，用两个set来做去重，然后存下unique字符的数量。最后遍历一遍，如果`left[i] == right[n - 1 - i]`，说明是一个good split，答案自增1.

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numSplits(string s) {

        unordered_set<int> s1, s2;
        int size = s.size();
        vector<int> v1(size, 0), v2(size, 0);

        for (int i = 0; i < size; i++) {
            s1.insert(s[i]);
            s2.insert(s[size - i - 1]);
            v1[i] = s1.size();
            v2[size - i - 1] = s2.size();
        }
        
        int result = 0;
        for (int i = 0; i < size - 1; i++) {
            if (v1[i] == v2[i + 1]) {
                result++;
            }
        }
        return result;
    }
private:
    
};
```

------

## Summary

&emsp;&emsp;这道题实际上是presum思路的一个扩展，presum并不是一种特别的算法，它的本质就是对重复计算的内容进行优化，这里就是对重复统计unique字符做一个优化，提前算好，实际使用的时候直接用就可以了。这道题目的分享到这里，感谢你的支持！
