---
title: LeetCode解题报告（322)-- 841. Keys and Rooms
tags:
  - LeetCode
mathjax: true
abbrlink: 38749
date: 2021-03-23 01:44:29
---

## Problem

There are `N` rooms and you start in room `0`.  Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`.  A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked (except for room `0`). 

You can walk back and forth between rooms freely.

Return `true` if and only if you can enter every room.

<!-- more -->

**Example 1:**

```
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

**Example 2:**

```
Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
```

**Note:**

1. `1 <= rooms.length <= 1000`
2. `0 <= rooms[i].length <= 1000`
3. The number of keys in all rooms combined is at most `3000`.

------

## Analysis

&emsp;&emsp;这是一道非常经典的BFS题目，题目大致如下：给定若干个房间，每个房间里面有若干条钥匙，默认第一个房间是打开的，问能不能打开所有的房间。其实转化为图问题，就是给定了起点，问能否到达图上所有的位置。这种题目直接BFS即可，记录下开过的房间，每次把新的钥匙都添加到队列中，遍历完成后，看看是否所有的房间都开过即可。

------

## Solution

&emsp;&emsp;需要用到一个`visited`数组记录每个房间是否被开过门。

------

## Code

```c++
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int size = rooms.size();
        queue<int> q;
        vector<bool> visited(size, false);
        q.push(0);
        visited[0] = true;
        
        while (!q.empty()) {
            int s = q.size();
            for (int i = 0; i < s; i++) {
                int idx = q.front();
                q.pop();
                for (auto &key: rooms[idx]) {
                    if (!visited[key]) {
                        visited[key] = true;
                        q.push(key);
                    }
                }
            }
        }
        for (int i = 0; i < size; i++) {
            if (!visited[i]) {
                return false;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是非常简单的BFS题目，主要还是依赖于queue来实现。这道题目的分享到这里，感谢你的支持！
