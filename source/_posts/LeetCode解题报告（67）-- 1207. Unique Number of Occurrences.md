---
title: LeetCode解题报告（67）-- 1207. Unique Number of Occurrences 
tags:
  - LeetCode
date: 2019-10-01 09:25:37


---

## Problem


Given an array of integers `arr`, write a function that returns `true` if and only if the number of occurrences of each value in the array is unique.

<!-- more -->

**Example 1:**

```
Input: arr = [1,2,2,1,1,3]
Output: true
Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.
```

**Example 2:**

```
Input: arr = [1,2]
Output: false
```

**Example 3:**

```
Input: arr = [-3,0,1,-3,1,1,1,-3,10,0]
Output: true
```

**Constraints**

1. `1 <= arr.length <= 1000`
2. `-1000 <= arr[i] <= 1000`

------

## Analysis

&emsp;&emsp;这道题是有关数字出现的频率的题目，一般这种题目都会用到map来解决问题。题目要求我们判断每个数字出现的次数是否唯一，那么可set来判断唯一性。

&emsp;&emsp;具体做法是，首先遍历给定的数字，利用map来做统计，然后对每一个数字出现的次数做唯一性判断，依次将他们放入set中，如果发现重复则返回`false`，反之返回`true`。

------

## Solution

&emsp;&emsp;这道题在思路和实现上没有太大的难度，主要是map和set的使用，文末我给出几个博客供大家参考。

------

## Code

```c++
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        map<int, int> m;
        int size = arr.size();
        for (int i = 0; i < size; i++) {
            m[arr[i]]++;
        }
        
        set<int> s;
        map<int, int>::iterator it;
        for (it = m.begin(); it != m.end(); it++) {
            if (s.find(it->second) != s.end()) {
                return false;
            }
            else {
                s.insert(it->second);
            }
        }
        return true;
    }
};
```

------

## Summary

 &emsp;&emsp;题目本身难度不大，可以视作是对map和set的练手题。熟练掌握STL的用法对于我们快速解题是非常有帮助的。这道题目的分享就到这里，谢谢您的支持！

------

## Reference

1. [c++ map常见用法](https://blog.csdn.net/shuzfan/article/details/53115922)
2. [c++ set常见用法](
