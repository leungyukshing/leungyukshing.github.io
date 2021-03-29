---
title: LeetCode解题报告（324)-- 966. Vowel Spellchecker
tags:
  - LeetCode
mathjax: true
date: 2021-03-27 16:40:03

---

## Problem

Given a `wordlist`, we want to implement a spellchecker that converts a query word into a correct word.

For a given `query` word, the spell checker handles two categories of spelling mistakes:

- Capitalization: If the query matches a word in the wordlist (

  case-insensitive

  ), then the query word is returned with the same case as the case in the wordlist.

  - Example: `wordlist = ["yellow"]`, `query = "YellOw"`: `correct = "yellow"`
  - Example: `wordlist = ["Yellow"]`, `query = "yellow"`: `correct = "Yellow"`
  - Example: `wordlist = ["yellow"]`, `query = "yellow"`: `correct = "yellow"`

- Vowel Errors: If after replacing the vowels

   

  ```
  ('a', 'e', 'i', 'o', 'u')
  ```

   

  of the query word with any vowel individually, it matches a word in the wordlist (

  case-insensitive

  ), then the query word is returned with the same case as the match in the wordlist.

  - Example: `wordlist = ["YellOw"]`, `query = "yollow"`: `correct = "YellOw"`
  - Example: `wordlist = ["YellOw"]`, `query = "yeellow"`: `correct = ""` (no match)
  - Example: `wordlist = ["YellOw"]`, `query = "yllw"`: `correct = ""` (no match)

In addition, the spell checker operates under the following precedence rules:

- When the query exactly matches a word in the wordlist (**case-sensitive**), you should return the same word back.
- When the query matches a word up to capitlization, you should return the first such match in the wordlist.
- When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
- If the query has no matches in the wordlist, you should return the empty string.

Given some `queries`, return a list of words `answer`, where `answer[i]` is the correct word for `query = queries[i]`.

<!-- more -->

**Example 1:**

```
Input: wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]
Output: ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]
```

**Example 2:**

```
Input: wordlist = ["yellow"], queries = ["YellOw"]
Output: ["yellow"]
```

**Constraints:**

- `1 <= wordlist.length, queries.length <= 5000`
- `1 <= wordlist[i].length, queries[i].length <= 7`
- `wordlist[i]` and `queries[i]` consist only of only English letters.

------

## Analysis

&emsp;&emsp;题目要求实现一个元音的拼写检查器，检查器可以允许两类错误：

1. 大小写错误，即大小写不影响两个单词之间的匹配；
2. 元音错误，即一个单词中元音字符可以相互匹配。

&emsp;&emsp;所以上面这两种情况都是可以成功匹配的，而且题目也给出了匹配的优先级，从高到低按照以下顺序排列：

1. 完全匹配，即两个字符串要完全一样；
2. 大小写匹配，即忽略大小写的不同，按照字母匹配；
3. 元音匹配，即忽略元音字符的不同；

&emsp;&emsp;我们先来看每一种类型的匹配怎么实现，然后再看这个优先级怎么做。第一种非常简单，我们用一个set或者map存储`wordlist`，然后对`queries`中每一个单词寻找一下是否存在即可。

&emsp;&emsp;第二种是大小写的匹配，因为是忽略大小写，所以直接统一处理成小写即可，所以可以先把`wordlist`中的单词都转换为小写存储到map中，然后对`queries`中每一个单词也转换为小写，然后再看看是否存在map中。

&emsp;&emsp;第三个是元音的匹配，如果参照第二种匹配的思路，应该就是把元音位置上所有的可能性都找出来，然后存到map中，最后在匹配的时候也是把每一种情况都列举出来。这样做是可行的，但是复杂度非常高。比如`google`这个单词，有三个元音字符，每个元音位置都有5种情况，所以一共会产生75个单词。这个数量太大了，我们要想办法优化。

&emsp;&emsp;有没有发现，按照上面这种做法，在预处理`wordlist`的时候我们要全部找一遍，然后在匹配`queries`中的单词的时候，我们还要全部找一遍，但是我们并不关心它最后能匹配到什么单词，所以元音位置上最终替换成什么我们是不关心的，那这里就参考了**正则表达式**的思想，用一个特殊字符代替元音就好。假设这里我用`*`来代替元音字符，那么上面例子的`google`就会成为`g**gl*`，这个就是存储的key，value是`google`。然后有一个待匹配的单词`gaegle`，我们也采取同样的处理方式，处理为`g**gl*`，这样就会发现这两个能匹配上了！

&emsp;&emsp;三种匹配我们都找到方法了，然后我们就要看优先级的问题。实际上因为这里优先级并不复杂，而且三种方法是各自独立的，为了解题的需要，每种匹配方法都各自使用独立的存储空间即可，在判断逻辑的时候有分先后就可以了。

------

## Solution

&emsp;&emsp;精确匹配因为key和value都是一样的，所以就用了set，后面两个就用map。

------

## Code

```c++
class Solution {
public:
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        vector<string> result;
        unordered_set<string> s;
        unordered_map<string, string> m1;
        unordered_map<string, string> m2;
        for (auto &word: wordlist) {
            s.insert(word);
            if (!m1.count(to_lower(word))) {
                m1[to_lower(word)] = word;            
            }
            int size = word.size();
            string str = to_reg(word);
            if (!m2.count(str)) {
                m2[str] = word;                
            }
        }
        
        for (auto &query: queries) {
            if (s.count(query)) {
                result.push_back(query);
            } else if (m1.count(to_lower(query))) {
                result.push_back(m1[to_lower(query)]);
            } else if (m2.count(to_reg(query))) {
                result.push_back(m2[to_reg(query)]);
            } else {
                result.push_back("");
            }
        }
        return result;
    }
private:
    string to_lower(string str) {
        string result = "";
        for (int i = 0; i < str.size(); i++) {
            result += tolower(str[i]);
        }
        return result;
    }
    
    string to_reg(string word) {
        word = to_lower(word);
        string str = "";
        int size = word.size();
        for (int i = 0; i < size; i++) {
            if (word[i] == 'a' 
                || word[i] == 'e' 
                || word[i] == 'i'
                || word[i] == 'o'
                || word[i] == 'u') {
                str += "*";
            } else {
                str += word[i];
            }
        }
        return str;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是字符串类题目中比较负责和综合的题目，实际上拆解开每个部分都不难，但是综合起来考虑还是需要对字符串比较熟悉才行。这道题目还有一个值得注意的点是，利用了正则的思想去做模糊匹配。这道题目的分享到这里，感谢你的支持！
