---
title: LeetCode解题报告（40）-- 287. Find the Duplicate Number
tags:
  - LeetCode
mathjax: true
abbrlink: 4518
date: 2018-12-03 13:40:28
---
## Problem
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.
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
  1. You must not modify the array (assume the array is read only).
  2. You must use only constant, O(1) extra space.
  3. Your runtime complexity should be less than $O(n^2)$.
  4. There is only one duplicate number in the array, but it could be repeated more than once.

## Analysis
&emsp;&emsp;这道题考点是数组的操作，题目要求我们找到一个数组中重复的数字，这个重复的数字是唯一的，但重复次数不限。这类题目难点不在于求解，而在于算法的时间复杂度与空间复杂度，这里要求使用O(1)的空间，和低于$O(n^2)$的时间复杂度。同时还有一条要求是不能修改原数组。我们针对这些要求逐个方法进行分析。
&emsp;&emsp;很容易的一种做法是对数组进行快速排序（这样时间复杂度就满足），得出的结果从小到大排列。然后直接遍历数组，对比相邻两个元素，如果相邻元素是一样的，则找到重复元素。这种做法在时间复杂度上是合格的，也没有用到额外的存储空间，但是它修改了数组本身。
&emsp;&emsp;然后我们来讨论一种优化的算法。我们可以在前面的一种算法上提高时间复杂度，思路还是一样的，我们只需要找到那个**不在自己位置上的元素**，不一定要通过排序才可以做到，只通过简单的交换也可以做到一样的效果。遍历数组`nums`，每个元素应该在的位置是`num[i]`应该在`nums[nums[i] - 1]`上，即`nums[i] = nums[nums[i] - 1]`。我们可以通过交换`nums[i]`和`nums[nums[i] - 1]`来将元素放到它该在的位置。由于有1-n个位置，而只有n个数字，而且还有重复的数字，那么肯定会有缺失的数字的位置被重复的数字占据了，因此只需要检查`nums[i] == i+1`，若不满足，则找到了这个占据了其他元素的数字，这个就是重复的数字了。这样时间复杂度就降到了O(n)。
&emsp;&emsp;可是这样还不是最满足题目要求的。我们如何要在不修改原有数组的情况下去实现呢？也就是说，我们只能遍历，然后就找到重复的数字了。这里要用到一个模型：将一个是数组抽象为一个环和一条线。详细原理分析以后再写（最近太忙了）。通过两个指针（1个快指针，1个慢指针，快指针一步跳一个，慢指针逐个遍历）进行遍历，当他们第一次相遇后，重置快指针为0，然后两个指针重新开始遍历（两个指针都是一步一个元素），最终相遇的点指向的就是重复的元素了。这种做法很巧妙，要充分了解了模型后才能运用，它没有修改原有数组的内容，整个过程也是对数组进行遍历，因此均满足题目的要求！

## Solution
&emsp;&emsp;根据分析的方法实现即可，本题在coding上没有什么难度。

## Code
方法1：
```C++
int findDuplicate(vector<int>& nums) {
  // Method 1
  int size = nums.size();
  sort(nums.begin(), nums.end());

  for (int i = 0; i < size; i++) {
    if (nums[i] == nums[i+1]) {
      return nums[i];
      }
  }
}
```

方法2：
```C++
int findDuplicate(vector<int>& nums) {
  int size = nums.size();
  for (int i = 0; i < size; i++) {
    if (nums[i] == i+1 || nums[i] == nums[nums[i] - 1])
      continue;
    swap(nums[i], nums[nums[i] - 1]);
    i--;
  }
  for (int i = 0; i < size; i++) {
    if (nums[i] != i+1) {
      return nums[i];
    }
  }
}
```

方法3：
```C++
int findDuplicate(vector<int>& nums) {
  // Method 3
  int slow = nums[0], fast = nums[nums[0]];
  while (slow != fast) {
    slow = nums[slow];
    fast = nums[nums[fast]];
  }
  fast = 0;
  while (slow != fast) {
    slow = nums[slow];
    fast = nums[fast];
  }
  return fast;
}
```

**运行时间：**约8ms，超过82.84%的CPP代码。

## Summary
&emsp;&emsp;这道题看上去是一道很简单的找重复元素的题目，题目难度是Medium，就算使用前两种做法也可以很快通过LeetCode的测试。但是如果自己对于算法要求比较高的话，不妨尝试一下，按照题目的要求进行设计。第三种方法非常巧妙，以后也可以使用到，我认为这题是非常好的题目，在这里和大家分享。如果有疑问或者建议，欢迎联系我，谢谢！
