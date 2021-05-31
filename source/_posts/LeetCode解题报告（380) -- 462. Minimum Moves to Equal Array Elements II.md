---
title: LeetCode解题报告（380) -- 462. Minimum Moves to Equal Array Elements II
tags:
  - LeetCode
mathjax: true
date: 2021-05-31 23:47:24

---

## Problem

Given an integer array `nums` of size `n`, return *the minimum number of moves required to make all array elements equal*.

In one move, you can increment or decrement an element of the array by `1`.

Test cases are designed so that the answer will fit in a **32-bit** integer.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3]
Output: 2
Explanation:
Only two moves are needed (remember each move increments or decrements one element):
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

**Example 2:**

```
Input: nums = [1,10,2,9]
Output: 16
```



**Constraints:**

- `n == nums.length`
- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组，每次操作只能对一个数+1或-1，问最少多少次操作后，能使得数组中所有的元素相等。

&emsp;&emsp;给出两个数，我们要求把这两个数变成相同的数，目标值肯定是在中间，而且中间任意一个数作为目标值，这两个数需要操作的次数都是一样的。例如两个数1和10，如果要都变成2，1需要操作1次，10需要操作8次，一共9次；如果都变成5，1需要操作4次，10需要操作5次，一共也是9次。因此操作次数是两数之差。

&emsp;&emsp;按照这个规律，我们只需要头尾两数相减，一直往中间走，把他们的差都加起来就是操作的数量。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int result = 0, i = 0, j = nums.size() - 1;
        sort(nums.begin(), nums.end());
        while (i < j) {
            result += (nums[j--] - nums[i++]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是一道比较简单的数组题，其涉及到的更多是基础数学。这道题目的分享到这里，感谢你的支持！
