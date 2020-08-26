---
title: LeetCode解题报告（114）-- 1032. Stream of Characters
tags:
  - LeetCode
mathjax: true
date: 2020-08-26 16:11:44


---

## Problem

Implement the `StreamChecker` class as follows:

- `StreamChecker(words)`: Constructor, init the data structure with the given words.
- `query(letter)`: returns true if and only if for some `k >= 1`, the last `k` characters queried (in order from oldest to newest, including this letter just queried) spell one of the words in the given list.

<!-- more -->

**Example:**

```
StreamChecker streamChecker = new StreamChecker(["cd","f","kl"]); // init the dictionary.
streamChecker.query('a');          // return false
streamChecker.query('b');          // return false
streamChecker.query('c');          // return false
streamChecker.query('d');          // return true, because 'cd' is in the wordlist
streamChecker.query('e');          // return false
streamChecker.query('f');          // return true, because 'f' is in the wordlist
streamChecker.query('g');          // return false
streamChecker.query('h');          // return false
streamChecker.query('i');          // return false
streamChecker.query('j');          // return false
streamChecker.query('k');          // return false
streamChecker.query('l');          // return true, because 'kl' is in the wordlist
```

**Note:**

- `1 <= words.length <= 2000`
- `1 <= words[i].length <= 2000`
- Words will only consist of lowercase English letters.
- Queries will only consist of lowercase English letters.
- The number of queries is at most 40000.

------

## Analysis

&emsp;&emsp;先来理解一下题目意思。首先给出了一个词典，然后不断接受用户的输入，在用户的输入流中，从新到旧的顺序去词典中寻找是否有匹配到的单词（匹配也是从后往前），返回匹配结果。

&emsp;&emsp;首先一看到题目就会觉得复杂度非常高了，一个是用户的输入是一个stream，所以每次都要从最新的开始从前匹配。第二个就是匹配，每次都要从用户的输入中截取出一段，然后和词典中每一个词进行匹配。

&emsp;&emsp;理解了为何这个问题这么复杂后，这里我直接给出有关字典匹配常用的一个方法——字典树Trie。字典树的作用是用树的形式存放字典，每个node存一个字符，一个单词就是一条从根节点到子节点的路径，但不一定到叶子节点（比如`app`和`apple`，第二个`p`不是叶子节点，但是匹配`app`的时候就是匹配完成了）。所以判断是否匹配完成就要在每一个node加上一个`isWordEnd`的布尔变量。

&emsp;&emsp;由于对用户输入流的处理是从新到旧，所以我们要记录下用户的历史输入，每次都从最新的开始拿。

------

## Solution

&emsp;&emsp;实现Trie其实就是构造一颗树，这里用vector来放每个节点的叶子节点指针，通过字符计算出下标来访问。

&emsp;&emsp;用户的输入流采用一个vector记录即可，每次把新的输入插入到最后。同时，由于匹配要求是从新到旧，所以在构建字典和处理用户输入流的时候都需要倒序处理，这样匹配的时候就是从根节点开始了。

------

## Code

```c++
class Node {
public:
    vector<Node*> children;
    bool isWordEnd;
    Node() {
        this->children = vector<Node*>(26);
        this->isWordEnd = false;
    }
};

class StreamChecker {
private:
    Node* root;
    vector<char> queryList;
public:
    StreamChecker(vector<string>& words) {
        this->root = new Node();
        int size = words.size();
        
        // construct trie
        for (int i = 0; i < size; i++) {
            Node* current = root;
            string word = words[i];
            int wordSize = word.size();
            for (int j = wordSize - 1; j >= 0; j--) {
                if (current->children[word[j] - 'a'] == NULL) {
                    current->children[word[j] - 'a'] = new Node();
                }
                current = current->children[word[j] - 'a'];
            }
            current->isWordEnd = true;
        }
    }
    
    bool query(char letter) {
        this->queryList.push_back(letter);
        
        int size = this->queryList.size();
        Node* p = root;
        for (int i = size - 1; i >= 0; i--) {
            char ch = this->queryList[i];
            if (p->children[ch - 'a'] == NULL) {
                return false;
            } else {
                p = p->children[ch - 'a'];
            }
            
            // match
            if (p->isWordEnd) {
                return true;
            }
        }
        return false;
    }
};
/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker* obj = new StreamChecker(words);
 * bool param_1 = obj->query(letter);
 */

```

------

## Summary

&emsp;&emsp;这道题的本质就是Trie的应用，也是希望借这道题去了解字典树的使用。实际上去构造Trie并不是很复杂，所以以后碰到字符串匹配的题目也可以考虑用Trie。这道题的分析到这里，谢谢！
