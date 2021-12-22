---
title: LeetCode解题报告（436）-- 1733. Minimum Number of People to Teach
mathjax: true
date: 2021-12-22 16:59:41
---

## Problem

On a social network consisting of `m` users and some friendships between users, two users can communicate with each other if they know a common language.

You are given an integer `n`, an array `languages`, and an array `friendships` where:

- There are `n` languages numbered `1` through `n`,
- `languages[i]` is the set of languages the `ith` user knows, and
- `friendships[i] = [ui, vi]` denotes a friendship between the users `ui` and `vi`.

You can choose **one** language and teach it to some users so that all friends can communicate with each other. Return *the* ***minimum*** *number of users you need to teach.*

Note that friendships are not transitive, meaning if `x` is a friend of `y` and `y` is a friend of `z`, this doesn't guarantee that `x` is a friend of `z`.

<!-- more -->

**Example 1:**

```
Input: n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
Output: 1
Explanation: You can either teach user 1 the second language or user 2 the first language.
```

**Example 2:**

```
Input: n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
Output: 2
Explanation: Teach the third language to users 1 and 3, yielding two users to teach.
```

**Constraints:**

- `2 <= n <= 500`
- `languages.length == m`
- `1 <= m <= 500`
- `1 <= languages[i].length <= n`
- `1 <= languages[i][j] <= n`
- `1 <= ui < vi <= languages.length`
- `1 <= friendships.length <= 500`
- All tuples `(ui, vi)` are unique
- `languages[i]` contains only unique values

------

## Analysis

&emsp;&emsp;题目给出三个信息，`n`代表语言的数量，`languages`代表每个人掌握的语言，`friendships`代表朋友关系。朋友关系中有些关系的两个人并没有掌握同一门语言，所以他们不能沟通。我们要做的就是教会这些人一门语言，使得朋友关系中的人都能和朋友交流，要求教的人最少。

&emsp;&emsp;这个题目看起来非常绕，但可以类比一下到生活中，就很好理解了。第一个问题是要教哪些人？这个范围`friendships`已经给出来了，如果两个人没有朋友关系，那我们不care；如果两个人有朋友关系但他们都掌握了同一门语言，我们也不care；我们只care朋友关系中两个人没有掌握同一门语言的情况，比如A只懂1和2，B只懂3和4，那么我最后肯定要么教A，要么教B，所以我把这些人存到一个map中。

&emsp;&emsp;然后问题就来到了我要教哪一门语言，才能使得教的人最少。这个是一个非常直观的问题，肯定是教懂的人最多的语言，懂的人多说明需要教的人就少，因此我们就统计上面那个map中，各种语言的掌握人数，并找到最多人掌握的那门语言。

&emsp;&emsp;最后就可以直接计算出答案了。`所有暂时不能和朋友沟通的人 - 都掌握了最多人掌握的语言的人`就是需要学习这门语言的人。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        unordered_map<int, int> notFriend;
        int size = friendships.size();
        for (vector<int> &f: friendships) {
            if (!canTalk(languages, f[0], f[1])) {
                notFriend[f[0]]++;
                notFriend[f[1]]++;
            }
        }
        
        unordered_map<int, int> languageFreq;
        int mostFreq = 0;
        for (auto &[k, v]: notFriend) {
            for (int &language: languages[k - 1]) {
                languageFreq[language]++;
                mostFreq = max(mostFreq, languageFreq[language]);
            }
        }
        return notFriend.size() - mostFreq;
    }
private:
    bool canTalk(vector<vector<int>>& language, int i, int j) {
        vector<int> v1 = language[i - 1];
        vector<int> v2 = language[j - 1];
        for (int &x: v1) {
            for (int &y: v2) {
                if (x == y) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目题目意思比较绕，但是逻辑上是非常贴切日常生活的。这道题目的分享到这里，感谢你的支持！
