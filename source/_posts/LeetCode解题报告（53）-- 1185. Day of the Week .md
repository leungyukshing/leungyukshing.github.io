---
title: LeetCode解题报告（53）-- 1185. Day of the Week
tags:
  - LeetCode
date: 2019-09-18 11:36:39

---

## Problem

Given a date, return the corresponding day of the week for that date.

The input is given as three integers representing the `day`, `month` and `year` respectively.

Return the answer as one of the following values:
`{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}`.

<!-- more -->

**Example 1:**

```
Input: day = 31, month = 8, year = 2019
Output: "Saturday"
```

**Example 2:**

```
Input: day = 18, month = 7, year = 1999
Output: "Sunday"
```

**Example 3:**

```
Input: day = 15, month = 8, year = 1993
Output: "Sunday"
```

**Constraints:**

- The given dates are valid dates between the years `1971` and `2100`.

------

## Analysis

&emsp;&emsp;这道题是与日期有关的题目，题目给出一个日期，要求我们给出改日期是当周的哪一天。我的做法比较直接，直接从1970年1月1号（Friday）开始算，算到给定的日期，再对7进行取余，这样我们就能够得到相对于Friday隔了多少天。

&emsp;&emsp;另外还有一个非常巧妙的方法，使用[蔡勒公式](https://www.cnblogs.com/igaoshang/articles/Zeller.html)。具体用法参考链接就可以，这里不再赘述。

------

## Solution

&emsp;&emsp;从年开始算，再到月，需要注意年份和月份都不需要算当前的，如2019年7月20日，年份的计算只需要算到2018，月份的计算只需要算到6月。同时还需要注意闰年的影响。最后计算出距离1970年1月1日的天数，因为当天是Friday，而我们需要将Sunday作为每周的第一天，所以需要在天数+2，再取余。

------

## Code

```c++
class Solution {
public:
    string dayOfTheWeek(int day, int month, int year) {
        int daysOfMonth[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        string dayName[] = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        
        int total = 1;
        for (int i = 1970; i < year; i++) {
            total += 365 + isLeapYear(i);
        }
        for (int i = 0; i < month - 1; i++) {
            if (i == 1) {
                total += daysOfMonth[i] + isLeapYear(year);
            }
            else {
                total += daysOfMonth[i];
            }
        }
        total += day;
        return dayName[(total + 2)% 7];
    }
private:
    bool isLeapYear(int year) {
        return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
    }
};
```

------

## Summary

&emsp;&emsp;这是一道比较综合性的日期类题目，如果使用python，可以有更加快速的做法。使用c++的话更能锻炼自己对日期方面的编程能力，主要是闰年、边界值这两个易错点。这道题目的分享到这里，欢迎大家转发、评论，谢谢您的支持！
