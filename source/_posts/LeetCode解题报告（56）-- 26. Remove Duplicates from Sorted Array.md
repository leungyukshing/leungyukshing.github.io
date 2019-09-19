---
title: LeetCode解题报告（56）-- 26. Remove Duplicates from Sorted Array
tags:
  - LeetCode
date: 2019-09-19 17:45:13
---

## Problem

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm)such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

<!-- more -->

**Example 1:**

```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

------

## Analysis

&emsp;&emsp;这道题是和数组有关的题目，题目的要求有点模糊，这里我用中文重新解释一遍，题目给出一个**有序数组**，要求我们返回唯一元素的数量`n`，**并且将传入的vector前n个元素调整为是唯一元素的数组**。觉得难理解的可以看**clarification**。

&emsp;&emsp; 题目要求不能使用额外的空间，所以想另外用一个set来存储的方法就不能行得通。由于题目传入的参数是引用，而且需要打印处理后的vector，所以我们只能在原有的vector上进行操作。我的做法是：逐个遍历元素，每找到一个新的唯一元素，我们就将它放在前面的位置，由于我们不需要care超出长度的元素是什么，所以直接赋值就可以了。

------

## Solution

&emsp;&emsp; 使用`mark`记录上一个遍历过的元素，用于确认当前元素是否新的唯一元素。再用一个`index`来记录下一个放置元素的位置，用于返回vector。

------

## Code

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int size = nums.size();
        if (size == 0) {
            return 0;
        }
        int mark = nums[0];
        int result = 1;
        int index = 1;
        for (int i = 1; i < size; i++) {
            if (mark == nums[i]) {
                continue;
            }
            else {
                mark = nums[i];
                nums[index++] = mark;
                result++;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道有关唯一元素的题目，常规来说都可以通过map或者set来解决，但是这道题需要我们返回vector，只能比较粗暴地遍历，然后一个一个元素塞进去。这道题的分享到这里，欢迎大家评论和分享，谢谢您的支持！
