---
title: LeetCode解题报告（241）-- 80. Remove Duplicates from Sorted Array II
tags:
  - LeetCode
mathjax: true
date: 2020-12-11 18:05:02

---

## Problem

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.

Do not allocate extra space for another array; you must do this by **modifying the input array in-place** with O(1) extra memory.

<!-- more -->

**Clarification:**

Confused why the returned value is an integer, but your answer is an array?

Note that the input array is passed in by **reference**, which means a modification to the input array will be known to the caller.

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

**Example 1:**

```
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3]
Explanation: Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively. It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3]
Explanation: Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively. It doesn't matter what values are set beyond the returned length.
```

**Constraints:**

- `0 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in ascending order.

------

## Analysis

&emsp;&emsp;题目给定一个数组，里面包含重复的数字，题目要求我们把出现次数超过两次的元素去掉，而且不能使用额外的空间，最后返回一个长度。这里有两个难点，一个出现超过两次的才去掉，第二个是不使用额外的空间。

&emsp;&emsp;我们先来解决第一个。前面做过的题目遇到统计出现次数的，一般都会考虑使用map，这里也是可以的，但是题目给出的数组是递增的，有了这个性质我们就不需要用map了。因为在处理完一个数字之后，就不需要再使用它的次数，因此这里用一个变量存就可以。

&emsp;&emsp;然后我们再来看第二个问题，因为不能够使用额外的存储空间，所以只能是交换的操作了。这里交换的肯定是**多余的数字**和后面的数字，所以要有两个下标表示。一个下标指向多余的数字，所以是一步一步走，另一个下标指向要替换的有效数字，所以有可能需要跳过一下多余的数字，这样看起来就是一对快慢指针了。

------

## Solution

&emsp;&emsp;用一个变量`count`去限制同一个数字出现的次数，初始化为1，当相同数字出现时，`count--`。当数字变化的时候重置为1。当相同数字出现并且`count`为0的时候，快指针就需要跳过这个多余的元素。

------

## Code

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slow = 0, fast = 1, count = 1, size = nums.size();
        while (fast < size) {
            if (nums[slow] == nums[fast] && count == 0) {
                fast++;
            } else {
                if (nums[slow] == nums[fast]) {
                    count--;
                } else {
                    count = 1;
                }
                nums[++slow] = nums[fast++];
            }
        }
        return size == 0 ? 0 : slow + 1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的本质还是数组的操作，在in-place的操作中，快慢指针是比较常用的手段。这道题目的分享到这里，谢谢您的支持！
