---
title: LeetCode解题报告（263）-- 127. Word Ladder
tags:
  - LeetCode
mathjax: true
date: 2021-01-12 01:11:20

---

## Problem

Given two words `beginWord` and `endWord`, and a dictionary `wordList`, return *the length of the shortest transformation sequence from* `beginWord` *to* `endWord`, *such that*:

- Only one letter can be changed at a time.
- Each transformed word must exist in the word list.

Return `0` if there is no such transformation sequence.

<!-- more -->

**Example 1:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog", return its length 5.
```

**Example 2:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: 0
Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
```

**Constraints:**

- `1 <= beginWord.length <= 100`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the strings in `wordList` are **unique**.

------

## Analysis

&emsp;&emsp;经典的Word Ladder题目，题目给出一个字典、开始词和结束词，每次只能够改变单词中的某个字符，而且只能用在字典里面的词语，要求找到从开始词到结束词的最短路径。

&emsp;&emsp;首先我们不考虑字典这个条件，假设现在所有的单词都可以使用，但是每次仍然是只能改变一个字符，那我们会怎么做呢？很直观的做法就是，对于单词中每个位置`word[i]`，都换成其余的25个字母试一下。以Example1为例，我们对`hit`第一个位置进行变换，得到`ait`、`bit`、`cit`等等，然后再对`ait`的第二个位置做同样的事情，这样最后就能找到答案。

&emsp;&emsp;然后我们加上字典这个条件，发现其实也不难，我们只需要在生成了新的单词后，查一下看看是不是在字典中，如果不是的话就忽略掉，只有在字典中我们后面才会继续处理，这无疑也减少了很多的运算。

&emsp;&emsp;但是我们还没有解决最关键的一个问题：**最短路径**。我们怎么保证找到的路径是最短的呢？路径的长度就是变换的次数，更改的位置数量越少，路径越短，而我们每一次只能更新一个位置，所以每一个位置就是一层，这就是常规的BFS！BFS的层数就是路径的长度，到了这里这道题目就非常简单了。

------

## Solution

&emsp;&emsp;题目给出的字段是数组，转换成set利于后面去查询。

------

## Code

```c++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> s(wordList.begin(), wordList.end());
        if (!s.count(endWord)) {
            return 0;
        }
        int wordLen = beginWord.size();
        queue<string> q;
        q.push(beginWord);
        
        int result = 0;
        // BFS
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                string word = q.front();
                q.pop();
                if (word == endWord) {
                    // reach endWord
                    return result + 1;
                }
                for (int j = 0; j < wordLen; j++) {
                    string str = word;
                    for (char ch = 'a'; ch <= 'z'; ch++) {
                        str[j] = ch;
                        if (s.count(str) && str != word) {
                            q.push(str);
                            s.erase(str);
                        }
                    }
                }
            }
            result++; // increment level
        }
        return 0;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是很经典的字符串问题，但其本质是图的BFS。单词中每一个位置都是相当于一个节点，邻居就是把该位置替换成其他25个字符，利用常规的BFS就能顺利求解。这道题这道题目的分享到这里，谢谢您的支持！
