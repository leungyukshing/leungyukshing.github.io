---
title: LeetCode解题报告（93）-- 287. Find the Duplicate Number
tags:
  - LeetCode
mathjax: true
abbrlink: 17104
date: 2020-06-28 16:14:22
---

## Problem

Given an array *nums* containing *n* + 1 integers where each integer is between 1 and *n* (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

<!-- more -->

**Example 1:**

```
Input: [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: [3,1,3,4,2]
Output: 3
```

**Note:**

1. You **must not** modify the array (assume the array is read only).
2. You must use only constant, *O*(1) extra space.
3. Your runtime complexity should be less than *O*(*n*2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

------

## Analysis

&emsp;&emsp;这道题目的意思是非常简单的，对于新手来说，使用双层循环就能解决了，时间复杂度是$O(n^2)$。当然这里我们要寻找更加高效的方式，很容易考虑的是二分搜索（**这里是对答案的二分**），题目说明给定的数字是在$[1,n]$，所以我们可以每次二分都统计比`mid`小的数字数量，如果这个数量大于等于`mid`，则说明重复的数比`mid`小；反之则说明重复的数比`mid`大。

&emsp;&emsp;用二分答案的方法已经能够满足题目的要求，但是这里我们寻求一种更加高效的方法——快慢指针。题目给出的是一个数组，但如果我们把每一个值看作是一个指针，那么因为有重复的数字，所以肯定有两个不同下标的值会指向同一个位置。这样就会形成一个环。我们先用快慢指针找到环，然后再用一个新的指针从头开始走，因为原来的慢指针一直在环里面，而另一个从头开始找，所以两个指针一定会找到相同的值，这个就是最终的结果了。

------

## Solution

&emsp;&emsp;快慢指针就用两个变量`slow`和`fast`表示就可以了，判断条件就是两个是否相等。

------

## Code

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0, t = 0;
        while (true) {
            slow = nums[slow];
            fast = nums[nums[fast]];
            if (slow == fast) break;
        }
        while (true) {
            slow = nums[slow];
            t = nums[t];
            if (slow == t) break;
        }
        return slow;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是非常典型的快慢指针的应用，巧妙的地方在于使用数组下标作为指针，从而找到环。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
