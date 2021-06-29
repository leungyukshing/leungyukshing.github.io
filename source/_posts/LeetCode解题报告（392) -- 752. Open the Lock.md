---
title: LeetCode解题报告（392) -- 752. Open the Lock
tags:
  - LeetCode
mathjax: true
abbrlink: 7515
date: 2021-06-13 23:35:06
---

## Problem

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

<!-- more -->

**Example 1:**

```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

**Example 2:**

```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".
```

**Example 3:**

```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation:
We can't reach the target without getting stuck.
```

**Example 4:**

```
Input: deadends = ["0000"], target = "8888"
Output: -1
```



**Constraints:**

- `1 <= deadends.length <= 500`
- `deadends[i].length == 4`
- `target.length == 4`
- target **will not be** in the list `deadends`.
- `target` and `deadends[i]` consist of digits only.

------

## Analysis

&emsp;&emsp;题目的背景是开锁，从`0000`开始到题目给定的`target`，同时还给出了几个`deadends`状态。这个其实本质上就是一个图的问题，和二维矩阵中的地图是一个道理。`deadends`就是不能走的地方，转移的状态就是每个位置上+1和-1（0减1变9，9加1变0）。

&emsp;&emsp;使用BFS解决，用`visited`记录已经访问过的状态，每次生成新的状态好，首先要判断是否已经访问过，同时还需要判断是否是`deadend`，两者都为否才放入queue中。

------

## Solution

&emsp;&emsp;这里处理状态转移有一个技巧，为了统一处理，可以先把数字加10，然后加上变化的+1或-1，然后再取个位数字。

------

## Code

```c++
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        string lock = "0000";
        unordered_set<string> deadlock(deadends.begin(), deadends.end());
        if (deadlock.count(lock)) {
            return -1;
        }
        unordered_set<string> visited({"0000"});
        if (visited.count(target)) {
            return 0;
        }
        queue<string> q;
        q.push(lock);
        int level = 0;
        
        while (!q.empty()) {
            level++;
            int size = q.size();
            for (int i = 0; i < size; i++) {
                string str = q.front();
                q.pop();
                
                for (int j = 0; j < str.size(); j++) {
                    for (int k = -1; k <= 1; k++) {
                        if (k == 0) {
                            continue;
                        }
                        string temp = str;
                        temp[j] = ((str[j] - '0') + 10 + k) % 10 + '0';
                        if (temp == target) {
                            return level;
                        }
                        if (!visited.count(temp) && !deadlock.count(temp)) {
                            q.push(temp);
                        }
                        visited.insert(temp);
                    }
                }
            }
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题的本质还是BFS，只不过背景换成了字符串有点陌生。这道题目的分享到这里，感谢你的支持！
