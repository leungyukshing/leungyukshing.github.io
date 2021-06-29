---
title: LeetCode解题报告（361) -- 970. Powerful Integers
tags:
  - LeetCode
mathjax: true
abbrlink: 62344
date: 2021-05-02 12:25:49
---

## Problem

Given three integers `x`, `y`, and `bound`, return *a list of all the **powerful integers** that have a value less than or equal to* `bound`.

An integer is **powerful** if it can be represented as `xi + yj` for some integers `i >= 0` and `j >= 0`.

You may return the answer in **any order**. In your answer, each value should occur **at most once**.

<!-- more -->

**Example 1:**

```
Input: x = 2, y = 3, bound = 10
Output: [2,3,4,5,7,9,10]
Explanation:
2 = 20 + 30
3 = 21 + 30
4 = 20 + 31
5 = 21 + 31
7 = 22 + 31
9 = 23 + 30
10 = 20 + 32
```

**Example 2:**

```
Input: x = 3, y = 5, bound = 15
Output: [2,4,6,8,10,14]
```

**Constraints:**

- `1 <= x, y <= 100`
- `0 <= bound <= 106`

------

## Analysis

&emsp;&emsp;题目给出两个数字`x`和`y`，然后还有一个上限`bound`，要求是使用`x`和`y`的次幂相加，和不超过`bound`，把所有的和都返回。这里我的思路也很简单，先把所有`x`和`y`的次幂数都计算出来，然后相互求和。

&emsp;&emsp;因为上限是`bound `，所以在计算`x `和`y`的次幂时，不能比这个大。分别计算出来后，再相加即可。

------

## Solution

&emsp;&emsp;这里还有一个细节需要处理，因为两个次幂数相加时，可能会产生重复的结果，这里我的做法是直接用set存储，最后再转化为vector返回。

------

## Code

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int size = nums.size();
        vector<int> result;
        int length = 0;
        bool flag = true;
        for (int i = 0; i < size; i++) {
            if (nums[i] == target) {
                if (flag) {
                    flag = false;
                    result.push_back(i);                    
                }
                length++;
            }
        }
        if (flag) {
            result.push_back(-1);
            result.push_back(-1);
        } else {
            result.push_back(result[0] + length - 1);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
