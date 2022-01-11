---
title: LeetCode解题报告（508）-- 1966. Binary Searchable Numbers in an Unsorted Array
mathjax: true
date: 2022-01-10 22:39:36
---

## Problem

Consider a function that implements an algorithm **similar** to [Binary Search](https://leetcode.com/explore/learn/card/binary-search/). The function has two input parameters: `sequence` is a sequence of integers, and `target` is an integer value. The purpose of the function is to find if the `target` exists in the `sequence`.

<!-- more -->

The pseudocode of the function is as follows:

```
func(sequence, target)
  while sequence is not empty
    randomly choose an element from sequence as the pivot
    if pivot = target, return true
    else if pivot < target, remove pivot and all elements to its left from the sequence
    else, remove pivot and all elements to its right from the sequence
  end while
  return false
```

When the `sequence` is sorted, the function works correctly for **all** values. When the `sequence` is not sorted, the function does not work for all values, but may still work for **some** values.

Given an integer array `nums`, representing the `sequence`, that contains **unique** numbers and **may or may not be sorted**, return *the number of values that are **guaranteed** to be found using the function, for **every possible** pivot selection*.



**Example 1:**

```
Input: nums = [7]
Output: 1
Explanation: 
Searching for value 7 is guaranteed to be found.
Since the sequence has only one element, 7 will be chosen as the pivot. Because the pivot equals the target, the function will return true.
```

**Example 2:**

```
Input: nums = [-1,5,2]
Output: 1
Explanation: 
Searching for value -1 is guaranteed to be found.
If -1 was chosen as the pivot, the function would return true.
If 5 was chosen as the pivot, 5 and 2 would be removed. In the next loop, the sequence would have only -1 and the function would return true.
If 2 was chosen as the pivot, 2 would be removed. In the next loop, the sequence would have -1 and 5. No matter which number was chosen as the next pivot, the function would find -1 and return true.

Searching for value 5 is NOT guaranteed to be found.
If 2 was chosen as the pivot, -1, 5 and 2 would be removed. The sequence would be empty and the function would return false.

Searching for value 2 is NOT guaranteed to be found.
If 5 was chosen as the pivot, 5 and 2 would be removed. In the next loop, the sequence would have only -1 and the function would return false.

Because only -1 is guaranteed to be found, you should return 1.
```



**Constraints:**

- `1 <= nums.length <= 105`
- `-105 <= nums[i] <= 105`
- All the values of `nums` are **unique**.

 

**Follow-up:** If `nums` has **duplicates**, would you modify your algorithm? If so, how?

---

## Analysis

&emsp;&emsp;这道题目给出一个无序数组`nums`，然后给出一个函数的算法，函数是对数组进行二分查找，找一个`target`。问这个数组中有多少个元素是可以一定被二分查找找到的。

&emsp;&emsp;我们先来看二分查找的过程，每次任意选一个`pivot`元素，如果小于`target`则移除包括自己在内左边所有的元素，否则则一处包括自己在内右边的所有元素。所以如果`target`的右边有一个比它小的元素，当`pivot`选到这个小的元素，`target`就会被移除，所以比`target`小的元素必须出现在`target`的左侧；同理，如果`target`的左边有一个比自己大的元素，当`pivot`选到这个大的元素，`target`就会被移除，所以比`target`大的元素必须出现在`target`的右侧。

&emsp;&emsp;所以总的来说我们就是要在`nums`中找一个单调递增的子序列，同时子序列中的每个元素都不能比前面出现过的元素（可以不在子序列中，只要是出现过就行）小。维持单调的序列我们通常可以用一个单调栈，为了保证新加入的元素不会比前面出现过的元素小，再维护一个当前最大值，只要比这个最大值大的元素才能入栈，最后返回栈的size。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int binarySearchableNumbers(vector<int>& nums) {
        stack<int> st; // increasing stack
        int maxValue = INT_MIN;
        for (int num: nums) {
            while (!st.empty() && st.top() > num) {
                st.pop();
            }
            if (num > maxValue) {
                st.push(num);
            }
            maxValue = max(maxValue, num);
        }
        return st.size();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上和二分查找关系不大，主要是单调栈的应用。同时有一个细节是加入栈的元素要比出现过的元素都要大这点很容易忽略。这道题目的分享到这里，感谢你的支持！

