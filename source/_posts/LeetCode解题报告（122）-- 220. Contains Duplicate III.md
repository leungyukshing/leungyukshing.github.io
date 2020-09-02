---
title: LeetCode解题报告（122）-- 220. Contains Duplicate III
tags:
  - LeetCode
mathjax: true
date: 2020-09-02 16:04:03

---

## Problem

Given an array of integers, find out whether there are two distinct indices *i* and *j* in the array such that the **absolute** difference between **nums[i]** and **nums[j]** is at most *t* and the **absolute** difference between *i* and *j* is at most *k*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

------

## Analysis

&emsp;&emsp;这道题目虽然是系列题，但是和前两个没什么关联，所以思路上也就没有办法参考前两题了。题目要求的是找出某个区间$[i, j]$，满足$|i - j | \le k$并且区间里面的任意两个元素的绝对值小于等于`t`。

&emsp;&emsp;因为是连续的区间问题，所以可以从区间入手，先定下区间，再验证里面的元素是否满足要求。确定区间只需要用两个下标就可以实现了，校验区间的长度是否满足要求，然后用一个数据结构保存这个区间里面数字。然后就是对这个数据结构里面的元素去做一个验证了。这里有一个前提就是，区间往后移动一定是因为前一个区间内的元素不满足要求（否则的话上一个验证成功就直接return了），所以只需要关注新加入的这个元素`nums[i]`。根据题目的要求要满足$|nums[i] - x| <= t$，其中`x`是在这个数据结构中的元素，那么这就转化为能否在map中找到这个`x`。上面的不等式展开为$nums[i] - t \le x \le t + nums[i]$，对于这个不等式我们分开两步来做判断。首先判断前半部分，这个标明了`x` 的下界，所以可以利用`lower_bound()`方法在这个数据结构中找到这个x，然后再判断这个x是否满足后半部分。

------

## Solution

&emsp;&emsp;可以看到在上面的分析中，存储区间元素使用的数据结构我一直没有指明，这也是这道题目巧妙的地方。按道理说用一个vector存放就可以了，但这里我推荐使用的是map，原因是我们用到的`lower_bound`方法。如果在vector中使用，就需要每次都对vector排序，但如果对map用，因为map本身是按key排序的，所以就不需要每次都排序。

&emsp;&emsp;另外数据类型方面也要使用`long long`类型，因为有部分的test case数据比较大，这个在题目中没有给出，有一点点坑。

------

## Code

```c++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        map<long long, int> m;
        int size = nums.size();
        
        int j = 0;
        for (int i = 0; i < size; i++) {
            if (i - j > k) {
                m.erase(nums[j++]);
            }
            auto x = m.lower_bound((long long)nums[i] - t);
            if (x != m.end() && (x->first - nums[i]) <= t) {
                return true;
            }
            m[nums[i]] = i;
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这种类型的题目我会归类为区间+内部判断的题目，这类题很重要的一个思路就是先定区间，再处理区间内的逻辑。同时在刷题的时候要学会熟练地运用STL的方法，如这里用到的`lower_bound`就是去找出满足条件的最小值。这道题的分析到这里，谢谢！
