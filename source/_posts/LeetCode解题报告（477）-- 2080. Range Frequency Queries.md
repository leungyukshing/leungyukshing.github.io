---
title: LeetCode解题报告（477）-- 2080. Range Frequency Queries
mathjax: true
date: 2021-12-29 17:23:32
---

## Problem

Design a data structure to find the **frequency** of a given value in a given subarray.

The **frequency** of a value in a subarray is the number of occurrences of that value in the subarray.

Implement the `RangeFreqQuery` class:

- `RangeFreqQuery(int[] arr)` Constructs an instance of the class with the given **0-indexed** integer array `arr`.
- `int query(int left, int right, int value)` Returns the **frequency** of `value` in the subarray `arr[left...right]`.

A **subarray** is a contiguous sequence of elements within an array. `arr[left...right]` denotes the subarray that contains the elements of `nums` between indices `left` and `right` (**inclusive**).

<!-- more -->

**Example 1:**

```
Input
["RangeFreqQuery", "query", "query"]
[[[12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]], [1, 2, 4], [0, 11, 33]]
Output
[null, 1, 2]

Explanation
RangeFreqQuery rangeFreqQuery = new RangeFreqQuery([12, 33, 4, 56, 22, 2, 34, 33, 22, 12, 34, 56]);
rangeFreqQuery.query(1, 2, 4); // return 1. The value 4 occurs 1 time in the subarray [33, 4]
rangeFreqQuery.query(0, 11, 33); // return 2. The value 33 occurs 2 times in the whole array.
```



**Constraints:**

- `1 <= arr.length <= 105`
- `1 <= arr[i], value <= 104`
- `0 <= left <= right < arr.length`
- At most `105` calls will be made to `query`

---

## Analysis

&emsp;&emsp;这道题目是一道设计题，要求是题目给出一个数组，然后有一个查询函数`query(int left, int right, int value)`，要求返回在`[left, right]`这个范围中`value`出现的次数。如果每次都把`[left,right]`遍历一遍，这个效率会很低，因此我们要换一种思路。

&emsp;&emsp;首先因为题目给出了`value`，我的思路就是能不能先用这个`value`减少一部分计算。我们可以用一个`unordered_map`存储每个`value`出现过的下标，存成一个数组。这样我需要处理的内容就不是`[left, right]`，而是一个记录了下标的数组。然后问题就变成如何在这个数组中找到`[left, right]`这个区间包含了多少元素。这个不就是左边界和有边界的问题吗？果断二分！

&emsp;&emsp;计算出左右边界之后相减就是出现的次数。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class RangeFreqQuery {
public:
    RangeFreqQuery(vector<int>& arr) {
        for (int i = 0; i < arr.size(); i++) {
            m[arr[i]].push_back(i);
        }
    }
    
    int query(int left, int right, int value) {
        if (!m.count(value)) {
            return 0;
        }
        vector<int> &arr = m[value];
        int l = 0, r = arr.size();
        
        // find left bound
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (left <= arr[mid]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        int left_bound = l;
        
        // find right bound
        l = 0, r = arr.size();
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (right >= arr[mid]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        int right_bound = r;
        //cout << left_bound << " " << right_bound << endl;
        return right_bound - left_bound;
    }
private:
    unordered_map<int, vector<int>> m;
};

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery* obj = new RangeFreqQuery(arr);
 * int param_1 = obj->query(left,right,value);
 */
```

------

## Summary

&emsp;&emsp;这道题目突破口在于，如何在一个记录下标的数组中找一个区间，这种做法是在设计题中是非常常见的。这道题目的分享到这里，感谢你的支持！

