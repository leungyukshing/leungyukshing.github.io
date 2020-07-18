---
title: LeetCode解题报告（101）-- 1344. Angle Between Hands of a Clock
tags:
  - LeetCode
date: 2020-07-18 14:57:02
mathjax: true

---

## Problem

Given two numbers, `hour` and `minutes`. Return the smaller angle (in degrees) formed between the `hour` and the `minute` hand.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/12/26/sample_1_1673.png)

```
Input: hour = 12, minutes = 30
Output: 165
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/12/26/sample_2_1673.png)

```
Input: hour = 3, minutes = 30
Output: 75
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2019/12/26/sample_3_1673.png)

```
Input: hour = 3, minutes = 15
Output: 7.5
```

**Example 4:**

```
Input: hour = 4, minutes = 50
Output: 155
```

**Example 5:**

```
Input: hour = 12, minutes = 0
Output: 0
```

**Constraints:**

- `1 <= hour <= 12`
- `0 <= minutes <= 59`
- Answers within `10^-5` of the actual value will be accepted as correct.

------

## Analysis

&emsp;&emsp;这道题目是关于时间角度计算的一道简单数学题，要求的时候分针和时针的角度差。首先一圈是360度，对分针来说，有60分钟，所以一分钟就是6度；对时针来说，有12小时，所以一小时就是30度。

&emsp;&emsp;当然这里还需要注意的点是当分针有偏移的时候，时针也会有相对应的偏移，所以在计算时针的角度的时候需要把这个偏移算上。

------

## Solution

&emsp;&emsp;分别计算时针和分针的偏移，然后取相减后的绝对值，最后用计算出较小的角度（`min(diff, 360 - diff)`）。

------

## Code

```c++
class Solution {
public:
    double angleClock(int hour, int minutes) {
        double minAngle = minutes * 6;
        double hourAngle = (hour % 12) * 30 + minutes * 30 / 60.0;
        
        // cout << hourAngle << " " << minAngle << endl;
        double diff = fabs(hourAngle - minAngle);
        // cout << diff << endl;
        return min(diff, 360 - diff);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难度不太大，主要就是一个比较简单的数学问题如何转化为代码，写的过程要注意除法的小数和角度。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
