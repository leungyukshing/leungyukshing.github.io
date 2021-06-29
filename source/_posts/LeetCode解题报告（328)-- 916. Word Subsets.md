---
title: LeetCode解题报告（328)-- 916. Word Subsets
tags:
  - LeetCode
mathjax: true
abbrlink: 16357
date: 2021-04-02 02:21:40
---

## Problem

We are given two arrays `A` and `B` of words.  Each word is a string of lowercase letters.

Now, say that word `b` is a subset of word `a` if every letter in `b` occurs in `a`, **including multiplicity**.  For example, `"wrr"` is a subset of `"warrior"`, but is not a subset of `"world"`.

Now say a word `a` from `A` is *universal* if for every `b` in `B`, `b` is a subset of `a`. 

Return a list of all universal words in `A`.  You can return the words in any order.

<!-- more -->

**Example 1:**

```
Input: A = ["amazon","apple","facebook","google","leetcode"], B = ["e","o"]
Output: ["facebook","google","leetcode"]
```

**Example 2:**

```
Input: A = ["amazon","apple","facebook","google","leetcode"], B = ["l","e"]
Output: ["apple","google","leetcode"]
```

**Example 3:**

```
Input: A = ["amazon","apple","facebook","google","leetcode"], B = ["e","oo"]
Output: ["facebook","google"]
```

**Example 4:**

```
Input: A = ["amazon","apple","facebook","google","leetcode"], B = ["lo","eo"]
Output: ["google","leetcode"]
```

**Example 5:**

```
Input: A = ["amazon","apple","facebook","google","leetcode"], B = ["ec","oc","ceo"]
Output: ["facebook","leetcode"]
```

**Note:**

1. `1 <= A.length, B.length <= 10000`
2. `1 <= A[i].length, B[i].length <= 10`
3. `A[i]` and `B[i]` consist only of lowercase letters.
4. All words in `A[i]` are unique: there isn't `i != j` with `A[i] == A[j]`.

------

## Analysis

&emsp;&emsp;题目给出`A`和`B`两个字符串数组，定义`A`中的某个单词`a`是**universal**如果所有在`B`中出现的字母都在`a`出现，包括出现的次数。因为这里有次数的要求，所以我们首先找到最低条件：**在B中每个字符都需要出现多少次**。例如`B = ["facebook", leetcode"]`，那么它`o`的最大出现次数就是2，来自于`facebook`这个单词。

&emsp;&emsp;然后我们再对`A`的单词逐个遍历，同样地计算出这个单词每个字母出现的次数，然后对比每个字母出现次数是否都大于等于B的最低要求，如果都满足，这个单词就是universal，加入到答案中；否则就检查下一个。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    vector<string> wordSubsets(vector<string>& A, vector<string>& B) {
        int size1 = A.size();
        int size2 = B.size();
        
        int maxCount[26] = {0};
        for (int i = 0; i < size2; i++) {
            vector<int> c = getCount(B[i]);
            for (int j = 0; j < 26; j++) {
                maxCount[j] = max(maxCount[j], c[j]);
            }
        }
        
        vector<string> result;
        for (int i = 0; i < size1; i++) {
            vector<int> c = getCount(A[i]);
            int j = 0;
            for (j = 0; j < 26; j++) {
                if (c[j] < maxCount[j]) {
                    break;
                }
            }
            if (j == 26) {
                result.push_back(A[i]);
            }
        }
        return result;
    }
private:
    vector<int> getCount(string str) {
        vector<int> result(26, 0);
        for (int i = 0; i < str.size(); i++) {
            result[str[i] - 'a']++;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的关键点在于找到判断universal的最低条件。因为每次都需要和`B`中所有的单词比较，所以可以先把这个最低条件求出来，然后每次只需要和这个条件做比较即可。这道题目的分享到这里，感谢你的支持！
