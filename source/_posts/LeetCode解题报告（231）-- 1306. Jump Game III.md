---
title: LeetCode解题报告（231）-- 1306. Jump Game III
tags:
  - LeetCode
mathjax: true
date: 2020-11-29 16:58:11

---

## Problem

Given an array of non-negative integers `arr`, you are initially positioned at `start` index of the array. When you are at index `i`, you can jump to `i + arr[i]` or `i - arr[i]`, check if you can reach to **any** index with value 0.

Notice that you can not jump outside of the array at any time.

<!-- more -->

**Example 1:**

```
Input: arr = [4,2,3,0,3,1,2], start = 5
Output: true
Explanation: 
All possible ways to reach at index 3 with value 0 are: 
index 5 -> index 4 -> index 1 -> index 3 
index 5 -> index 6 -> index 4 -> index 1 -> index 3 
```

**Example 2:**

```
Input: arr = [4,2,3,0,3,1,2], start = 0
Output: true 
Explanation: 
One possible way to reach at index 3 with value 0 is: 
index 0 -> index 4 -> index 1 -> index 3
```

**Example 3:**

```
Input: arr = [3,0,2,1,2], start = 2
Output: false
Explanation: There is no way to reach at index 1 with value 
```

**Constraints:**

- `1 <= arr.length <= 5 * 10^4`
- `0 <= arr[i] < arr.length`
- `0 <= start < arr.length`

------

## Analysis

&emsp;&emsp;这道题是数组中判断可达性的题目，题目先给定一个下标作为搜索的起点，定义移动规则：对于某个位置`i`，下一步只能移动到`i + arr[i]`或`i - arr[i]`，然后问若干步之后，能否到达值为0的位置。

&emsp;&emsp;这类题目很直接的做法就是搜索了，深度搜索或者宽度搜索都是常用的方法。这里我采用了宽度搜索，搜索过的位置就不再搜索。找到值为0的位置就return true，如果最后都没找到的话就return false。

------

## Solution

&emsp;&emsp;使用一个visited数组记录每个位置是否被遍历过，其他的和正常的BFS一样。

------

## Code

```c++
class Solution {
public:
    bool canReach(vector<int>& arr, int start) {
        int size = arr.size();
        vector<bool> visited(size, false);
        queue<int> q;
        
        if (arr[start] == 0) {
            return true;
        }
        q.push(start);
        visited[start] = true;
        
        while (!q.empty()) {
            int s = q.size();
            for (int i = 0; i < s; i++) {
                int idx = q.front();
                q.pop();
                // left
                int left = idx - arr[idx];
                if (left >= 0 && left < size && visited[left] == false) {
                    if (arr[left] == 0) {
                        return true;
                    } else {
                        visited[left] = true;
                        q.push(left);
                    }
                }
                // right
                int right = idx + arr[idx];
                if (right >= 0 && right < size && visited[right] == false) {
                    if (arr[right] == 0) {
                        return true;
                    } else {
                        visited[right] = true;
                        q.push(right);
                    }
                }
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目其实就是在数组中应用BFS，之前对二叉树进行BFS比较多，这次对数组也做BFS，大部分都是一样的，不同之处在于数组的移动是根据题目的要求进行，而二叉树一般是往叶子节点寻找。这道题目的分享到这里，谢谢您的支持！
