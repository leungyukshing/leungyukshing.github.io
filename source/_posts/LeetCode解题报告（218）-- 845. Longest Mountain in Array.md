---
title: LeetCode解题报告（218）-- 845. Longest Mountain in Array
tags:
  - LeetCode
mathjax: true
abbrlink: 50641
date: 2020-11-18 01:12:11
---

## Problem

Let's call any (contiguous) subarray B (of A) a *mountain* if the following properties hold:

- `B.length >= 3`
- There exists some `0 < i < B.length - 1` such that `B[0] < B[1] < ... B[i-1] < B[i] > B[i+1] > ... > B[B.length - 1]`

(Note that B could be any subarray of A, including the entire array A.)

Given an array `A` of integers, return the length of the longest *mountain*. 

Return `0` if there is no mountain.

<!-- more -->

**Example 1:**

```
Input: [2,1,4,7,3,2,5]
Output: 5
Explanation: The largest mountain is [1,4,7,3,2] which has length 5.
```

**Example 2:**

```
Input: [2,2,2]
Output: 0
Explanation: There is no mountain.
```

**Note:**

1. `0 <= A.length <= 10000`
2. `0 <= A[i] <= 10000`

**Follow up:**

- Can you solve it using only one pass?
- Can you solve it in `O(1)` space?

------

## Analysis

&emsp;&emsp;这道题目是和数组的遍历相关的，要求我们在数组中找到最长的peak。所谓peak就是数字从小到大，再到小。一个peak其实是由三个关键的位置组成的，起点`i`、终点`j`和山峰`peak`。我们每次移动这三个点到合适的位置，然后计算出这座peak的长度，维护一个最大的值即可。

&emsp;&emsp;那么现在问题就变成了，这三个点要怎么移动呢？

- 起点`i`。起点要保证尽可能地小，所以是向右寻找最小的；
- 山峰`peak`。山峰是数组大小突变的地方，从起点开始寻找，因为起点是极小值，所以山峰就是一个极大值，往右一直寻找最大的；
- 终点`j`。终点是这个山峰往右最小的，从山峰开始寻找，往右找到最小的。

------

## Solution

&emsp;&emsp;按照上面分析的思路依此移动三个点，每定位到一个山峰后，就计算这个山峰的长度，全局维护一个最大值即可。

------

## Code

```c++
class Solution {
public:
    int longestMountain(vector<int>& A) {
        int result = 0;
        int i = 0, size = A.size();
        while (i < size - 1) {
            while (i < size - 1 && A[i] >= A[i + 1]) {
                i++;
            }
            int peak = i;
            while (peak < size - 1 && A[peak] < A[peak + 1]) {
                peak++;
            }
            int j = peak;
            while (j < size - 1 && A[j] > A[j + 1]) {
                j++;
            }
            if (i < peak && peak < j) {
                result = max(result, j - i + 1);
            }
            i = j;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是数组遍历的一个变种，是基于数字大小关系的。这一类题目的突破点一半就在于大小关系的变化。数组类题目很多情况下都是要先确定下标，找到位置了题目也就迎刃而解了。这道题目的分享到这里，谢谢！
