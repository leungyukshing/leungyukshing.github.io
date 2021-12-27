---
title: LeetCode解题报告（462）-- 737. Sentence Similarity II
mathjax: true
date: 2021-12-27 10:30:04
---

## Problem

We can represent a sentence as an array of words, for example, the sentence `"I am happy with leetcode"` can be represented as `arr = ["I","am",happy","with","leetcode"]`.

Given two sentences `sentence1` and `sentence2` each represented as a string array and given an array of string pairs `similarPairs` where `similarPairs[i] = [xi, yi]` indicates that the two words `xi` and `yi` are similar.

Return `true` *if `sentence1` and `sentence2` are similar, or* `false` *if they are not similar*.

Two sentences are similar if:

- They have **the same length** (i.e., the same number of words)
- `sentence1[i]` and `sentence2[i]` are similar.

Notice that a word is always similar to itself, also notice that the similarity relation is transitive. For example, if the words `a` and `b` are similar, and the words `b` and `c` are similar, then `a` and `c` are **similar**.

<!-- more -->

**Example 1:**

```
Input: sentence1 = ["great","acting","skills"], sentence2 = ["fine","drama","talent"], similarPairs = [["great","good"],["fine","good"],["drama","acting"],["skills","talent"]]
Output: true
Explanation: The two sentences have the same length and each word i of sentence1 is also similar to the corresponding word in sentence2.
```

**Example 2:**

```
Input: sentence1 = ["I","love","leetcode"], sentence2 = ["I","love","onepiece"], similarPairs = [["manga","onepiece"],["platform","anime"],["leetcode","platform"],["anime","manga"]]
Output: true
Explanation: "leetcode" --> "platform" --> "anime" --> "manga" --> "onepiece".
Since "leetcode is similar to "onepiece" and the first two words are the same, the two sentences are similar.
```

**Example 3:**

```
Input: sentence1 = ["I","love","leetcode"], sentence2 = ["I","love","onepiece"], similarPairs = [["manga","hunterXhunter"],["platform","anime"],["leetcode","platform"],["anime","manga"]]
Output: false
Explanation: "leetcode" is not similar to "onepiece".
```



**Constraints:**

- `1 <= sentence1.length, sentence2.length <= 1000`
- `1 <= sentence1[i].length, sentence2[i].length <= 20`
- `sentence1[i]` and `sentence2[i]` consist of lower-case and upper-case English letters.
- `0 <= similarPairs.length <= 2000`
- `similarPairs[i].length == 2`
- `1 <= xi.length, yi.length <= 20`
- `xi` and `yi` consist of English letters.

---

## Analysis

&emsp;&emsp;这是一道系列题，这个系列的第一题比较简单，因为`similarPairs`里面的单词的相似性是不会传递的，因此我们用一个`unordered_map<string, unordered_set<int>>`就能记录到每个单词它和谁相似。而这里的第二题就是相似性会传递的，如果我们把单词看成一个点，把相似性看成一条边，那么如果某两个单词相似，它们就会在同一个连通分量里面，这种动态维护连通性的问题，指向也很明确了，就是用并查集。

&emsp;&emsp;并查集的实现这里不细说，主要说怎么使用。对于字符串类型来说，我们需要把单词映射到一个数字，这个数字从0开始，因此我们要用一个`unordered_map`去维护字符串和对应编号的映射，然后在并查集中我们使用编号去进行运算。把`similarPairs`中的单词都添加到并查集中，然后，同时遍历`sentence1`和`sentence2`，如果有不一样的字符并且不在同一个连通分量中，直接return false。

## Solution

&emsp;&emsp;需要特别注意两个单词相等的时候，也是相似的，不要漏了这种情况。

------

## Code

```c++
class Solution {
public:
    bool areSentencesSimilarTwo(vector<string>& sentence1, vector<string>& sentence2, vector<vector<string>>& similarPairs) {
        int size0 = sentence1.size(), size1 = sentence2.size();
        if (size0 != size1) {
            return false;
        }
        
        unordered_map<string, int> m; // word to idx
        int idx = 0;
        for (vector<string> &pair: similarPairs) {
            if (!m.count(pair[0])) {
                //cout << pair[0] << "->" << idx << endl;
                m[pair[0]] = idx++;
            }
            if (!m.count(pair[1])) {
                //cout << pair[1] << "->" << idx << endl;
                m[pair[1]] = idx++;
            }
        }
        
        parent = vector<int>(idx, 0);
        for (int i = 0; i < idx; i++) {
            parent[i] = i;
        }
        
        for (vector<string> &pair: similarPairs) {
            //cout << "connect " << pair[0] << " " << pair[1] << endl;
            unio(m[pair[0]], m[pair[1]]);
        }
        
        for (int i = 0; i < size0; ++i) {
            if (sentence1[i] != sentence2[i]) {
                //cout << sentence1[i] << " " << sentence2[i] << endl;
                //cout << find(m[sentence1[i]]) << " " << find(m[sentence2[i]]) << endl;
                if (!m.count(sentence1[i]) || !m.count(sentence2[i]) || find(m[sentence1[i]]) != find(m[sentence2[i]])) {
                    return false;
                }
            }
        }
        return true;
    }
private:
    vector<int> parent;
    
    int find(int x) {
        if (x != parent[x]) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    void unio(int x, int y) {
        parent[find(x)] = find(y);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是并查集的一个应用，稍微有不同的地方就是这是对字符串使用并查集，要多一步映射。这道题目的分享到这里，感谢你的支持！
