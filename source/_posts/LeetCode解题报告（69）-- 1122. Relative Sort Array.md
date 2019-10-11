---
title: LeetCode解题报告（69）-- 1122. Relative Sort Array
tags:
  - LeetCode
date: 2019-10-11 17:43:34
---

## Problem

Given two arrays `arr1` and `arr2`, the elements of `arr2` are distinct, and all elements in `arr2` are also in `arr1`.

Sort the elements of `arr1` such that the relative ordering of items in `arr1` are the same as in `arr2`.  Elements that don't appear in `arr2` should be placed at the end of `arr1` in **ascending**order.

<!-- more -->

**Example 1:**

```
Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]
```

**Constraints:**

- `arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- Each `arr2[i]` is distinct.
- Each `arr2[i]` is in `arr1`.

------

## Analysis

&emsp;&emsp;这道题是有关数组的题目，要求是题目给出两个数组`arr1`和`arr2`，题目需要将`arr1`中的元素按照`arr2`中的顺序进行排序。如果元素不在`arr2`中，则按照升序排序后放在数组的末尾。

&emsp;&emsp;我的做法非常直接，我首先使用map来记录`arr1`中每个数字出现的次数，然后按照`arr2`来遍历，根据map统计的次数来构成数组。每遍历完一个都将其删除，这样最后剩下的就是只出现在`arr1`中的数字。然后排序输出即可。

------

## Solution

&emsp;&emsp;这里主要使用了map，包括插入和删除操作。因为map的key是有序的，所以最后我们不需要额外进行排序操作。

------

## Code

```c++
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        vector<int> result;
        map<int, int> m;
        // traverse arr1
        int size1 = arr1.size();
        for (int i = 0; i < size1; i++) {
            m[arr1[i]]++;
        }
        
        int size2 = arr2.size();
        for (int i = 0; i < size2; i++) {
            result.insert(result.end(), m[arr2[i]], arr2[i]);
            m.erase(arr2[i]);
        }
        
        for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
            result.insert(result.end(), it->second, it->first);
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这类题目的做法往往五花八门，我的思路是借用map来简化实现的过程，当然也有其他的方法能够做到。这道题目的分享到这里，欢迎大家转发、评论，感谢您的支持！