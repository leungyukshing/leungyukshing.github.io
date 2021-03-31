---
title: LeetCode解题报告（326)-- 870. Advantage Shuffle
tags:
  - LeetCode
mathjax: true
date: 2021-03-31 01:56:49

---

## Problem

Given two arrays `A` and `B` of equal size, the *advantage of A with respect to B* is the number of indices `i` for which `A[i] > B[i]`.

Return **any** permutation of `A` that maximizes its advantage with respect to `B`.

<!-- more -->

**Example 1:**

```
Input: A = [2,7,11,15], B = [1,10,4,11]
Output: [2,11,7,15]
```

**Example 2:**

```
Input: A = [12,24,8,32], B = [13,25,32,11]
Output: [24,32,8,12]
```

**Note:**

1. `1 <= A.length = B.length <= 10000`
2. `0 <= A[i] <= 10^9`
3. `0 <= B[i] <= 10^9`

------

## Analysis

&emsp;&emsp;题目给出了两个数组`A`和`B`，定义A对B的优势值为`A[i] > B[i]`的位置数量，题目要求我们通过改变`A`的顺序使得优势值最大。这样一看，不就是典型的**田忌赛马**？

&emsp;&emsp;田忌赛马的基本思想就是用**恰好强的马去取胜，实在打不过的找个最菜的去应付。**换到这道题目中也是一样的，我们只需要找到恰好比`B[i]`大的数即可，这里的恰好意思是在`A`中找出最小的满足`A[i] > B[i]`的值，也就是upper_bound。如果某个对于`B[i]`，在`A`中都找不出数比它大，那就是打不过了，找个最小的去应付即可。

------

## Solution

&emsp;&emsp;这道题目需要找`upper_bound`，所以首先需要对`A`进行排序。然后找upper_bound使用stl的方法即可。

------

## Code

```c++
class Solution {
public:
    vector<int> advantageCount(vector<int>& A, vector<int>& B) {
        vector<int> res;
		multiset<int> st(A.begin(), A.end());
		for (int i = 0; i < B.size(); ++i) {
			auto it = (*st.rbegin() <= B[i]) ? st.begin() : st.upper_bound(B[i]);
			res.push_back(*it);
			st.erase(it);
		}
		return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，读懂题目意思后，其实就是很典型的田忌赛马问题。然后利用STL提供的方法来求解即可。这道题目的分享到这里，感谢你的支持！
