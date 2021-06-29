---
title: LeetCode解题报告（252）-- 1345. Jump Game IV
tags:
  - LeetCode
mathjax: true
abbrlink: 12069
date: 2020-12-28 09:51:12
---

## Problem

Given an array of integers `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index `i` to index:

- `i + 1` where: `i + 1 < arr.length`.
- `i - 1` where: `i - 1 >= 0`.
- `j` where: `arr[i] == arr[j]` and `i != j`.

Return *the minimum number of steps* to reach the **last index** of the array.

Notice that you can not jump outside of the array at any time.

<!-- more -->

**Example 1:**

```
Input: arr = [100,-23,-23,404,100,23,23,23,3,404]
Output: 3
Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.
```

**Example 2:**

```
Input: arr = [7]
Output: 0
Explanation: Start index is the last index. You don't need to jump.
```

**Example 3:**

```
Input: arr = [7,6,9,6,9,6,9,7]
Output: 1
Explanation: You can jump directly from index 0 to index 7 which is last index of the array.
```

**Example 4:**

```
Input: arr = [6,1,9]
Output: 2
```

**Example 5:**

```
Input: arr = [11,22,7,7,7,7,7,7,7,22,13]
Output: 3
```

**Constraints:**

- `1 <= arr.length <= 5 * 10^4`
- `-10^8 <= arr[i] <= 10^8`

------

## Analysis

&emsp;&emsp;这道题是在一个数组上进行搜索，移动有三种选择：向左、向右，还有跳到值相同的位置上，要求用最少的步数跳到最后一个元素。这道题目本质上是一个图，每个位置都是一个节点，他的邻居就是他能移动的方向。因此我们只需要用BFS就能找到最短的路径。

------

## Solution

&emsp;&emsp;因为向左和向右都是下标直接操作，所以不需要记录，而要事先记录的是值相同的坐标，这里用到map，key是值，value是**下标数组**。当需要找这个值的邻居时，把数组中所有的下标都添加到队列中，并且**删除掉**，因为之后也用不了这个值了。

------

## Code

```c++
class Solution {
public:
    int minJumps(vector<int>& arr) {
        int size = arr.size();
        unordered_map<int, vector<int>> m; // <value, idx>
        for (int i = 0; i < size; i++) {
            m[arr[i]].push_back(i);
        }
        
        vector<bool> visited(size);
        visited[0] = true;
        queue<int> q; // queue of index
        q.push(0);
        int result = 0;
        while (!q.empty()) {
            int s = q.size();
            while (s--) {
                int idx = q.front();
                q.pop();
                if (idx == size - 1) {
                    return result;
                }
                if (idx - 1 >= 0 && !visited[idx - 1]) {
                    visited[idx - 1] = true;
                    q.push(idx - 1);
                }
                if (idx + 1 < size && !visited[idx + 1]) {
                    visited[idx + 1] = true;
                    q.push(idx + 1);
                }
                auto iter = m.find(arr[idx]);
                if (iter == m.end()) {
                    // not found
                    continue;
                } else {
                    for (int next: iter->second) {
                        if (!visited[next]) {
                            visited[next] = true;
                            q.push(next);
                        }
                    }
                    m.erase(iter);
                }
            }
            result++;
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是在字符串的基础上进行BFS，是两个比较经典的知识点的一个创新结合。只要把整个过程转换为graph的BSF，coding就非常简单了。这道题目的分享到这里，谢谢您的支持！
