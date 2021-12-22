---
title: LeetCode解题报告（432）-- 843. Guess the Word
mathjax: true
date: 2021-12-22 12:38:25
---

## Problem

This is an ***interactive problem\***.

You are given an array of **unique** strings `wordlist` where `wordlist[i]` is `6` letters long, and one word in this list is chosen as `secret`.

You may call `Master.guess(word)` to guess a word. The guessed word should have type `string` and must be from the original list with `6` lowercase letters.

This function returns an `integer` type, representing the number of exact matches (value and position) of your guess to the `secret` word. Also, if your guess is not in the given wordlist, it will return `-1` instead.

For each test case, you have exactly `10` guesses to guess the word. At the end of any number of calls, if you have made `10` or fewer calls to `Master.guess` and at least one of these guesses was `secret`, then you pass the test case.

<!-- more -->

**Example 1:**

```
Input: secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"], numguesses = 10
Output: You guessed the secret word correctly.
Explanation:
master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
We made 5 calls to master.guess and one of them was the secret, so we pass the test case.
```

**Example 2:**

```
Input: secret = "hamada", wordlist = ["hamada","khaled"], numguesses = 10
Output: You guessed the secret word correctly.
```

**Constraints:**

- `1 <= wordlist.length <= 100`
- `wordlist[i].length == 6`
- `wordlist[i]` consist of lowercase English letters.
- All the strings of `wordlist` are **unique**.
- `secret` exists in `wordlist`.
- `numguesses == 10`

------

## Analysis

&emsp;&emsp;这是一道很有意思的题目。题目给出一个`wordlist`，里面包含若干个长度为6的字符串，同时还有一个`Master`的obj，要求我们调用`master.guess`去猜某个词，函数返回猜的词和正确的结果中匹配的字符个数，也就是说，如果猜对了就是返回6。题目只给10次调用机会。

&emsp;&emsp;首先线性的方法肯定是不可行的，因为一旦`wordlist`的长度超过10，一个一个猜肯定超过调用限制。我们要想办法每次都减少猜的范围。我们可以利用的信息是`guess`返回的匹配数量，这是一个非常重要的信息。如果猜的词和正确的词匹配的数量是`count`，那么我只需要在`wordlist`中找出其余的和猜的词匹配的数量是`count`的词，那么正确的那个词肯定在里面。

&emsp;&emsp;上面这个是一个很不错的优化思路，每次过后都能够把匹配数量不一致的词排除掉，但是还是跑不过test case，那么问题出在哪呢？我尝试打印了`count`，发现failed的case都是0，说明我们猜的词，一开始选的就有问题，选了匹配度为0的词，意味着wordlist中根本就没有排除掉很多单词。这里可以做一个简单的概率计算，和答案匹配度为0的单词的概率是$(\frac{25}{26})^6$，这个概率非常大，也就是说我们随便一猜猜到是匹配度为0的单词的概率高达80%。所以我们就从这里入手，我们首先计算出wordlist中两两单词的匹配度，统计每个单词和其他单词匹配度为0的数量。我们想要猜的是和其他单词匹配度为0最少的那个单词，这意味我有更大的概率能够猜到与答案相近的词。

&emsp;&emsp;通过上面第二个优化，代码就能通过test case了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */
class Solution {
public:
    void findSecretWord(vector<string>& wordlist, Master& master) {
        int count = 0;
        for (int i = 0; i < 10; ++i) {
            unordered_map<string, int> m1;
            for (string &w1: wordlist) {
                for (string &w2: wordlist) {
                    if (helper(w1, w2) == 0) {
                        m1[w1]++;
                    }
                }
            }

            // find the word with least 0 count
            pair<string, int> minWord = make_pair(wordlist[0], 1000);
            for (string &word: wordlist) {
                if (m1[word] <= minWord.second) {
                    minWord = {word, m1[word]};
                }
            }
            
            string candidate = minWord.first;
            count = master.guess(candidate);
            if (count == 6) {
                return;
            }
            
            vector<string> temp;
            for (string &word: wordlist) {
                if (helper(word, candidate) == count) {
                    temp.push_back(word);
                }
            }
            wordlist = temp;
        }
    }
private:
    int helper(string &s1, string &s2) {
        int result = 0;
        for (int i = 0; i < 6; ++i) {
            if (s1[i] == s2[i]) {
                ++result;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目题型比较新颖，是类似于调api的题目，这类题目的优化一般有两个方向，一个是通过二分查找进行优化，另一个就是通过一些贪心的思路去优化。这道题目的分享到这里，感谢你的支持！
