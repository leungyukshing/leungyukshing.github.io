---
title: LeetCode解题报告（257）-- 1640. Check Array Formation Through Concatenation
tags:
  - LeetCode
mathjax: true
abbrlink: 40985
date: 2021-01-04 02:34:52
---

## Problem

You are given an array of **distinct** integers `arr` and an array of integer arrays `pieces`, where the integers in `pieces` are **distinct**. Your goal is to form `arr` by concatenating the arrays in `pieces` **in any order**. However, you are **not** allowed to reorder the integers in each array `pieces[i]`.

Return `true` *if it is possible* *to form the array* `arr` *from* `pieces`. Otherwise, return `false`.

<!-- more -->

**Example 1:**

```
Input: arr = [85], pieces = [[85]]
Output: true
```

**Example 2:**

```
Input: arr = [15,88], pieces = [[88],[15]]
Output: true
Explanation: Concatenate [15] then [88]
```

**Example 3:**

```
Input: arr = [49,18,16], pieces = [[16,18,49]]
Output: false
Explanation: Even though the numbers match, we cannot reorder pieces[0].
```

**Example 4:**

```
Input: arr = [91,4,64,78], pieces = [[78],[4,64],[91]]
Output: true
Explanation: Concatenate [91] then [4,64] then [78]
```

**Example 5:**

```
Input: arr = [1,3,5,7], pieces = [[2,4,6,8]]
Output: false
```

**Constraints:**

- `1 <= pieces.length <= arr.length <= 100`
- `sum(pieces[i].length) == arr.length`
- `1 <= pieces[i].length <= arr.length`
- `1 <= arr[i], pieces[i][j] <= 100`
- The integers in `arr` are **distinct**.
- The integers in `pieces` are **distinct** (i.e., If we flatten pieces in a 1D array, all the integers in this array are distinct).

------

## Analysis

&emsp;&emsp;这道题目是数组类题目的一个变种，题目给出`arr`和`pieces`，问能否使用`pieces`里面的元素去构成`arr`。这道题目的限制比较严格，比如说元素的数量是刚好相等的，所以不用考虑使用一次或者多次的问题，同时每个数字都是唯一的，这些严格的限制给我们结题带来了很大的方便。

&emsp;&emsp;要判断能否构成，我们要知道每个`piece`在`arr`中的位置，这个可以通过`piece`的第一个元素来定位。又因为元素都是唯一的，所以可以直接记录每个`pieces[i][0]`在`arr`中的位置。然后再遍历`arr`，如果`arr[i]`是某个`piece`的第一个元素，那么就从这个`piece`往后一一对比；如果不是，则说明没有以这个值开头的`piece`，直接return false。

------

## Solution

&emsp;&emsp;记录`pieces[i][0]`和`arr`对应下标关系使用map结构即可。需要注意的是，在比对`arr`和`pieces`的过程中，一旦根据map定位到了元素，两个结构的下标都要同时往后移动。

------

## Code

```c++
class Solution {
public:
    bool canFormArray(vector<int>& arr, vector<vector<int>>& pieces) {
        int size = arr.size(), pieces_num = pieces.size();
        unordered_map<int, int> m;
        
        for (int i = 0; i < pieces_num; i++) {
            m[pieces[i][0]] = i;
        }
        
        for (int i = 0; i < size;) {
            int current = arr[i];
            if (m.count(current) == 0) {
                return false;
            }
            
            auto nums = pieces[m[current]];
            for (int j = 0; j < nums.size(); j++, i++) {
                if (arr[i] != nums[j]) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是数组类型的匹配题目，因为题目限制了许多edge case，所以求解起来也比较简单。这道题这道题目的分享到这里，谢谢您的支持！
