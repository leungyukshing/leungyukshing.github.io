---
title: LeetCode解题报告（224）-- 804. Unique Morse Code Words
tags:
  - LeetCode
mathjax: true
date: 2020-11-22 20:39:30


---

## Problem

GInternational Morse Code defines a standard encoding where each letter is mapped to a series of dots and dashes, as follows: `"a"` maps to `".-"`, `"b"` maps to `"-..."`, `"c"` maps to `"-.-."`, and so on.

For convenience, the full table for the 26 letters of the English alphabet is given below:

```
[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]
```

Now, given a list of words, each word can be written as a concatenation of the Morse code of each letter. For example, "cab" can be written as "-.-..--...", (which is the concatenation "-.-." + ".-" + "`-...`"). We'll call such a concatenation, the transformation of a word.

Return the number of different transformations among all words we have.

<!-- more -->

**Example 1:**

```
Input: words = ["gin", "zen", "gig", "msg"]
Output: 2
Explanation: 
The transformation of each word is:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."

There are 2 different transformations, "--...-." and "--...--.".
```

**Note:**

- The length of `words` will be at most `100`.
- Each `words[i]` will have length in range `[1, 12]`.
- `words[i]` will only consist of lowercase letters.

------

## Analysis

&emsp;&emsp;比较简单的字符串题目，题目给出一系列字符串，然后要求我们转换成摩斯密码。由于不同字符的组合最终有可能会产生相同的密码，所以题目要求计算出最后不同的密码数量。我们提前用一个数组存好字符和密码的转换，然后一次遍历就可以转换成密码了。因为是计算不同的密码，使用set来做去重即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        string table[] = {".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};
        set<string> s;
        int size = words.size();
        for (int i = 0; i < size; i++) {
            string temp = "";
            for (int j = 0; j < words[i].size(); j++) {
                temp += table[words[i][j] - 'a'];
            }
            s.insert(temp);
        }
        return s.size();
    }
    
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢！
