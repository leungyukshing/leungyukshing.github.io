---
title: LeetCode解题报告（100）-- 347. Top K Frequent Elements
tags:
  - LeetCode
date: 2020-07-18 14:00:48
mathjax: true


---

## Problem

Given a non-empty array of integers, return the **k** most frequent elements.

<!-- more -->

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Note:**

- You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
- Your algorithm's time complexity **must be** better than O(*n* log *n*), where *n* is the array's size.
- It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
- You can return the answer in any order.

------

## Analysis

&emsp;&emsp;这道题目是关于数字频率统计的，这种统计常规的做法是用map统计。而这道题目比较难的是有一个排序，要找出出现频率前k个高的。既然用到排序，就是用priority_queue，以出现的频率作为key，以数字本身作为value。插入到queue里面后，就已经排好序了，之后输出前k个就可以了。

------

## Solution

&emsp;&emsp;直接使用C++提供的unorder_map和priority_queue即可。

------

## Code

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> m;
        priority_queue<pair<int, int>> q;
        vector<int> res;
        
        int size = nums.size();
        
        for (int i = 0; i < size; i++) {
            m[nums[i]]++;
        }
        
        for (auto it : m) q.push({it.second, it.first});
        for (int i = 0; i < k; ++i) {
            res.push_back(q.top().second);
            q.pop();
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度是medium，实际的思路不难，但要求对一些数据结构的使用比较熟悉。如果对C++的一些stl容器比较熟悉的话，这道题就能快速解决了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
