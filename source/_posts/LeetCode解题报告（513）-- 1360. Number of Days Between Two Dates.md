---
title: LeetCode解题报告（513）-- 1360. Number of Days Between Two Dates
mathjax: true
date: 2022-01-12 22:31:24
---

## Problem

Write a program to count the number of days between two dates.

The two dates are given as strings, their format is `YYYY-MM-DD` as shown in the examples.

<!-- more -->

**Example 1:**

```
Input: date1 = "2019-06-29", date2 = "2019-06-30"
Output: 1
```

**Example 2:**

```
Input: date1 = "2020-01-15", date2 = "2019-12-31"
Output: 15
```



**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.

---

## Analysis

&emsp;&emsp;有了上一道题打了基础，面对这种日期相关的题目我们有了模板就很方便了。这道题目给出两个日期字符串，问两个日期相隔了多少天。如果直接计算相差了多少天非常麻烦，但是如果我们都转化为这个日期与1971年1月1号相差了多少天，最后把数量相减就很简单了。所以我们把问题实际上转化为给定一个日期，算距离1971年1月1号有多少天。

&emsp;&emsp;之前的题目是计算该日期在当年过了多少天，这里加上了年份，所以月份和天的计算是一样的，只用加上年份的计算即可。一年里面要么是365，要么是366，判断闰年的函数都是一样的，从1971年开始到当前`year`的前一年都计算一边，再加上当前年份过了多少天，就是答案了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int daysBetweenDates(string date1, string date2) {
        return abs(helper(date1) - helper(date2));
    }
private:
    int helper(string date) {
        int days[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        int year = stoi(date.substr(0, 4)), month = stoi(date.substr(5, 2)), day = stoi(date.substr(8));
        for (int y = 1971; y < year; ++y) {
            day += isLeap(y) ? 366: 365;
        }
        return day + (month > 2 && isLeap(year)) + accumulate(begin(days), begin(days) + month - 1, 0);
    }
    
    bool isLeap(int year) {
        return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
    }
};
```

------

## Summary

&emsp;&emsp;有了上一题的铺垫，这道题目就相对比较容易了，只需要在上一题的基础上加上年份的计算即可。这就是模板（套路）的魅力。这道题目的分享到这里，感谢你的支持！

