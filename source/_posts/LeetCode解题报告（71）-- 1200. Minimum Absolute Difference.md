---
title: LeetCode解题报告（70）-- 1002. Find Common Characters
tags:
  - LeetCode
date: 2019-10-11 17:59:12
---

## Problem

Given an array of **distinct** integers `arr`, find all pairs of elements with the minimum absolute difference of any two elements. 

Return a list of pairs in ascending order(with respect to pairs), each pair `[a, b]` follows

- `a, b` are from `arr`
- `a < b`
- `b - a` equals to the minimum absolute difference of any two elements in `arr`

<!-- more -->

**Example 1:**

```
Input: arr = [4,2,1,3]
Output: [[1,2],[2,3],[3,4]]
Explanation: The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.
```

**Example 2:**

```
Input: arr = [1,3,6,10,15]
Output: [[1,3]]
```

**Exaample 3:**

```
Input: arr = [3,8,-10,23,19,-4,-14,27]
Output: [[-14,-10],[19,23],[23,27]]
```

**Constraints:**

- `2 <= arr.length <= 10^5`
- `-10^6 <= arr[i] <= 10^6`

------

## Analysis

&emsp;&emsp;这道题要求我们找出差最小的数字组合。最愚蠢的做法当然是双重循环遍历，这里就不解释了。我的做法是，首先通过排序来减少循环的次数，排序后最小的差只会在相邻的两个元素之间出现。然后我们进行一次遍历就能找出最小的差。

&emsp;&emsp;最后我们还要返回所有能够有最小差的pair，我们可以再次遍历数组，如果相邻的数字能够得到这个最小的差，那么这一pair就是我们需要的。

------

## Solution

&emsp;&emsp;主要是排序和vector的基本操作，没有特别多的难点。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int size = arr.size();
        int minDiff = INT_MAX;
        for (int i = 0; i < size - 1; i++) {
            if ((arr[i + 1] - arr[i]) < minDiff) {
                minDiff = arr[i + 1] - arr[i];
            }
        }
        
        vector<vector<int>> result;
        for (int i = 0; i < size - 1; i++) {
            if ((arr[i + 1] - arr[i]) == minDiff) {
                vector<int> temp;
                temp.push_back(arr[i]);
                temp.push_back(arr[i + 1]);
                result.push_back(temp);
            }
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;我们可以从这道题学到，很多数组的题目是可以通过排序来优化的。这道题的分享就到这里，欢迎大家评论、转发，感谢你的支持！