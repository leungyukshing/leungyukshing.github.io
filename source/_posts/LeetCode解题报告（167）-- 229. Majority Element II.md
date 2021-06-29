---
title: LeetCode解题报告（167）-- 229. Majority Element II
tags:
  - LeetCode
mathjax: true
abbrlink: 54547
date: 2020-09-22 17:46:49
---

## Problem

Given an integer array of size *n*, find all elements that appear more than `⌊ n/3 ⌋` times.

**Note:** The algorithm should run in linear time and in O(1) space.

<!-- more -->

**Example 1:**

```
Input: [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

------

## Analysis

&emsp;&emsp;这道题目的意思本身是很简单的，给定一个数组，找出在这个数组中出现次数超过`n/3`的数字。但是题目给了时间和空间复杂度的限制条件，时间复杂度为`O(n)`，空间复杂度是常数，所以既不能排序，也不能用map。难度一下子就提上来了。

&emsp;&emsp;我们先来看看出现次数超过`n/3`的数字有多少个，这个是摩尔投票定理的拓展，实际上这个数字最多会有两个。简单说明一下，如果有3个数字的出现次数都大于`n/3`，那么这三个数字构成的数组长度就已经超过`n`了，还没有算上其他数字的，所以这是矛盾的。因此这道题目的答案不会超过两个。

&emsp;&emsp;分析到这里，我们的思路就剩下在数组中找出这两个数（2个是最多的情况，当然也有可能只有1个）。我们采用选举对票的机制，先选出出现次数最多的两个数字，然后再对这两个数字的出现次数做一次确认就可以了。

------

## Solution

&emsp;&emsp;这道题目在选举对票的时候比较有技巧，首先我们用两个变量`nums1`和`nums2`来记录两个候选的数组，用`count1`和`count2`来分别记录他们出现的次数。循环遍历的时候首先检查是否匹配这两个候选数字，如果匹配则增加他们对应的次数，否则，就减少他们出现的次数，这个就是对票（抵消）。如果某个数字的出现次数为0了，就说明他没有票了，所以就由当前遍历到的这个元素去做候选人。

&emsp;&emsp;这样选出来之后，这两个数就是出现次数最多的数。但是这并不保证他的出现次数都是超过`n/3`，所以我们再做一次遍历验证一下就可以了。

------

## Code

```c++
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int num1 = 0, num2 = 0, count1 = 0, count2 = 0;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            } else if (count1 == 0) {
                num1 = nums[i];
                count1++;
            } else if (count2 == 0) {
                num2 = nums[i];
                count2++;
            } else {
                count1--;
                count2--;
            }
        }
        
        count1 = count2 = 0;
        for (int i = 0; i < size; i++) {
            if (nums[i] == num1) {
                count1++;
            } else if (nums[i] == num2) {
                count2++;
            }
        }
        
        vector<int> result;
        if (count1 > size / 3) {
            result.push_back(num1);
        }
        if (count2 > size / 3) {
            result.push_back(num2);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题如果没有时间和空间的限制，处理起来会非常简单。但是加上限制之后，我们就需要从数学的角度出发先去分析，然后巧妙地使用摩尔投票的方式来进行统计，这两个点都是非常值得总结和分享的。这道题的分析到这里，谢谢！
