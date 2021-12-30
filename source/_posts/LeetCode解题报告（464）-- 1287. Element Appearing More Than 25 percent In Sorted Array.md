---
title: LeetCode解题报告（464）-- 1287. Element Appearing More Than 25% In Sorted Array
mathjax: true
date: 2021-12-27 21:38:19
---

## Problem

Given an integer array **sorted** in non-decreasing order, there is exactly one integer in the array that occurs more than 25% of the time, return that integer.

<!-- more -->

**Example 1:**

```
Input: arr = [1,2,2,6,6,6,6,7,10]
Output: 6
```

**Example 2:**

```
Input: arr = [1,1]
Output: 1
```



**Constraints:**

- `1 <= arr.length <= 104`
- `0 <= arr[i] <= 105`

---

## Analysis

&emsp;&emsp;题目给出了一个有序数组，要求找出其中出现超过25%的数。最简单的做法就是用一个`unordered_map`统计每个数字出现的次数，然后对比一下是否超过25%即可，但这种算法的时间复杂度和空间复杂度都是$O(n)$。虽然这种解法也能解决这道easy题，但我想用一种更优的方法去解决。

&emsp;&emsp;一般题目给出的数组是有序的，大多数情况都在暗示使用二分查找，但这里我还不知道找什么。突破点就在这个25%，我们把数组切成4等份，将会有三个切分的点`n / 4`、`n / 2`、`3n / 4`，这三个点其中必有一个是答案，为什么呢？因为答案是超过了25%，它一定会是这三个中的一个，所以我们只需要对这三个值做二分查找，分别查找出左边界和右边界，然后就能知道个数了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
        int size = arr.size();
        vector<int> candidates{arr[size / 4], arr[size / 2], arr[3 * size / 4]};
        
        for (int cand: candidates) {
            // find left bound
            int left = 0, right = size;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (cand <= arr[mid]) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            
            int left_bound = left;
            // find right bound
            left = 0, right = size;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (cand >= arr[mid]) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            int right_bound = right - 1;
            if ((right_bound - left_bound + 1) * 4 > size) {
                return cand;
            }
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然是easy，有简单的做法，但也有使用二分的做法，是一道比较不错的题目，同样也是考察了使用二分查找寻找左边界和右边界这两个知识点。这道题目的分享到这里，感谢你的支持！
