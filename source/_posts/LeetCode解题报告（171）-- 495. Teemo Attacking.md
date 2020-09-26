---
title: LeetCode解题报告（171）-- 495. Teemo Attacking
tags:
  - LeetCode
mathjax: true
date: 2020-09-26 23:06:12


---

## Problem

In LOL world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking **ascending** time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.

You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.

<!-- more -->

**Example 1:**

```
Input: [1,4], 2
Output: 4
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned immediately. 
This poisoned status will last 2 seconds until the end of time point 2. 
And at time point 4, Teemo attacks Ashe again, and causes Ashe to be in poisoned status for another 2 seconds. 
So you finally need to output 4.
```

**Example 2:**

```
Input: [1,2], 2
Output: 3
Explanation: At time point 1, Teemo starts attacking Ashe and makes Ashe be poisoned. 
This poisoned status will last 2 seconds until the end of time point 2. 
However, at the beginning of time point 2, Teemo attacks Ashe again who is already in poisoned status. 
Since the poisoned status won't add up together, though the second poisoning attack will still work at time point 2, it will stop at the end of time point 3. 
So you finally need to output 3.
```

**Note:**

1. You may assume the length of given time series array won't exceed 10000.
2. You may assume the numbers in the Teemo's attacking time series and his poisoning time duration per attacking are non-negative integers, which won't exceed 10,000,000.

------

## Analysis

&emsp;&emsp;题目给出技能的冷却时间和使用技能的时间点，要求计算出技能使用的总时间。这里的关键点是使用的时间是不能叠加的，所以在使用技能的时候，有两种情况：

1. 上一个技能已经使用完毕
2. 上一个技能还在使用中

&emsp;&emsp;对于上面第一种情况，我们只需要把当前技能的使用时间加上就可以了，对于第二种情况，实际上上一个技能的生效时间是当前的时间点减去上一个技能使用的时间点。所以只有通过下一个技能的时候，才能判断当前技能实际的生效时间。

------

## Solution

&emsp;&emsp;因为遍历的时候需要考虑到下一个技能使用，所以循环到`n-1`就可以了，因为最后一个技能使用的时间肯定是完整的，后面不会有其他的使用了。

------

## Code

```c++
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        int size = timeSeries.size();
        if (size == 0) {
            return 0;
        }
        int current = 0;
        int result = 0;
        for (int i = 0; i < size - 1; i++) {
            if (timeSeries[i] + duration > timeSeries[i + 1]) {
                result += timeSeries[i + 1] - timeSeries[i];;
            } else {
                result += duration;
            }
        }
        result += duration;
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题难度不大，梳理好两种情况的话，计算起来并不是很困难。这道题的分析到这里，谢谢！
