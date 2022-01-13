---
title: LeetCode解题报告（512）-- 1154. Day of the Year
mathjax: true
date: 2022-01-12 22:23:05
---

## Problem

Given a string `date` representing a [Gregorian calendar](https://en.wikipedia.org/wiki/Gregorian_calendar) date formatted as `YYYY-MM-DD`, return *the day number of the year*.

<!-- more -->

**Example 1:**

```
Input: date = "2019-01-09"
Output: 9
Explanation: Given date is the 9th day of the year in 2019.
```

**Example 2:**

```
Input: date = "2019-02-10"
Output: 41
```



**Constraints:**

- `date.length == 10`
- `date[4] == date[7] == '-'`, and all other `date[i]`'s are digits
- `date` represents a calendar date between Jan 1st, 1900 and Dec 31th, 2019.

---

## Analysis

&emsp;&emsp;接下来两道题目将讨论一下和日期相关的题目，希望能够通过两道题目把和数日子问题彻底解决。这道题目以字符串的方式给出了一个日期，问这个日期是该年的第几天。

&emsp;&emsp;首秀因为这个日期字符串是有格式要求的，年份4位，月份2位，日期2位，中间都用`-`相连，所以我们可以直接用`substr()`加上`stoi()`得到`year`、`month`和`day`。然后重点就是处理闰年了，闰年的条件是`year % 4 == 0 && (year % 100 != 0 || year % 400 == 0)`。只有当月份大于2（从3开始）才需要考虑闰年，其余的直接相加即可。所以这里最快的方法就是先写好一个数组，用`accumulate`的方法加到`month - 1`的月份，然后加上`day`就是答案。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int dayOfYear(string date) {
        int days[12] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        int year = stoi(date.substr(0, 4)), month = stoi(date.substr(5, 2)), day = stoi(date.substr(8));
        return day + (month > 2 && isLeap(year)) + accumulate(begin(days), begin(days) + month - 1, 0);
    }
private:
    bool isLeap(int year) {
        return year % 4 == 0 && (year % 100 != 0 || year % 400 == 0);
    }
};
```

------

## Summary

&emsp;&emsp;数日子的问题主要就是闰年的处理，方法有很多，这里我给出了一个相对简洁的套路。每个人都有自己的模板，如果没有模板这类题目处理起来代码会很复杂，有模板的话相对来说就会简单。这道题目的分享到这里，感谢你的支持！

