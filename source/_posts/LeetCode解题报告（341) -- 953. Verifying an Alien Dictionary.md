---
title: LeetCode解题报告（341) -- 953. Verifying an Alien Dictionary
tags:
  - LeetCode
mathjax: true
date: 2021-04-14 02:16:48

---

## Problem

In an alien language, surprisingly they also use english lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.

Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographicaly in this alien language.

<!-- more -->

**Example 1:**

```
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.
```

**Example 2:**

```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

**Example 3:**

```
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
```

**Constraints:**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `order.length == 26`
- All characters in `words[i]` and `order` are English lowercase letters.

------

## Analysis

&emsp;&emsp;题目给出了一个字符串数组，以及一个包含26个英文字母的字符串。后者定义了一种**新的字典序**，要求判断前面给出的字符串数组是否符合新的字典序。我们先来看，如果按照常规的26个英文字母的顺序，那么这道题就很简单了，我们只需要对前面的数组排序，然后判断一下排序前后是否一致就知道了。

&emsp;&emsp;那相同的思路能否应用到这道题目中呢？答案当然是可以的，我们只需要重定义排序的函数即可。我们比较大小的根本是比较两个字符在字典序中的下标大小，所以简单起见，先把字典序转化为一个map，通过字母去找到它在字典序中的下标。

&emsp;&emsp;然后就是自定义的比较函数了。从前往后逐个对比大小，如果相等再往后看，如果不想等就马上得到大小结果了。这里还要考虑长度不一致的情况，长度不一致时，如果前面的部分都相同，则较长的字符串较大。

------

## Solution

&emsp;&emsp;这里涉及到一个C++的知识点，在lambda表达式中，我们是通过捕获的方式把外面的变量传递进去，但是这里我用到了map。lambda表达式在捕获到map或者unordered_map时，会默认转化为const map或const unordered_map，这样的后果是在lambda表达式中无法使用`[]`运算符来访问元素。我这里的解决方法是使用`at()`方法替代。

------

## Code

```c++
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        map<char, int> m;
        int size = order.size();
        
        for (int i = 0; i < size; i++) {
            m[order[i]] = i;
        }
        
        for (int i = 0; i < words.size(); i++) {
            cout << words[i] << endl;
        }
        vector<string> words2 = words;
        sort(words.begin(), words.end(), [m](const string &a, const string &b) {
            int size1 = a.size();
            int size2 = b.size();
            
            int i = 0, j = 0;
            while (i < size1 && j < size2) {
                char ch1 = a[i];
                char ch2 = b[i];
                int idx1 = m.at(ch1);
                int idx2 = m.at(ch2);
                if (idx1 > idx2) {
                    return false;
                } else if (idx1 < idx2) {
                    return true;
                } else {
                    i++;
                    j++;
                }
            }
            
            while (i < size1) {
                return false;
            }
            
            while (j < size2) {
                return true;
            }
            return true;
        });
        
        // for (int i = 0; i < words.size(); i++) {
        //     cout << words[i] << endl;
        // }
        return words == words2;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，主要的知识点是coding方面的。主要是lambda表达式的应用，通过自定义比较函数重写sort。同时也踩了一下map的坑，估计很多不熟悉的朋友也会猜踩到这个坑。这道题目的分享到这里，感谢你的支持！
