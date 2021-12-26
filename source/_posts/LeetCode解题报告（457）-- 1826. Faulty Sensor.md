---
title: LeetCode解题报告（457）-- 1826. Faulty Sensor
mathjax: true
date: 2021-12-26 00:07:24
---

## Problem

An experiment is being conducted in a lab. To ensure accuracy, there are **two** sensors collecting data simultaneously. You are given two arrays `sensor1` and `sensor2`, where `sensor1[i]` and `sensor2[i]` are the `ith` data points collected by the two sensors.

However, this type of sensor has a chance of being defective, which causes **exactly one** data point to be dropped. After the data is dropped, all the data points to the **right** of the dropped data are **shifted** one place to the left, and the last data point is replaced with some **random value**. It is guaranteed that this random value will **not** be equal to the dropped value.

- For example, if the correct data is `[1,2,**3**,4,5]` and `3` is dropped, the sensor could return `[1,2,4,5,**7**]` (the last position can be **any** value, not just `7`).

We know that there is a defect in **at most one** of the sensors. Return *the sensor number (*`1` *or* `2`*) with the defect. If there is **no defect** in either sensor or if it is **impossible** to determine the defective sensor, return* `-1`*.*

<!-- more -->

**Example 1:**

```
Input: sensor1 = [2,3,4,5], sensor2 = [2,1,3,4]
Output: 1
Explanation: Sensor 2 has the correct values.
The second data point from sensor 2 is dropped, and the last value of sensor 1 is replaced by a 5.
```

**Example 2:**

```
Input: sensor1 = [2,2,2,2,2], sensor2 = [2,2,2,2,5]
Output: -1
Explanation: It is impossible to determine which sensor has a defect.
Dropping the last value for either sensor could produce the output for the other sensor.
```

**Example 3:**

```
Input: sensor1 = [2,3,2,2,3,2], sensor2 = [2,3,2,3,2,7]
Output: 2
Explanation: Sensor 1 has the correct values.
The fourth data point from sensor 1 is dropped, and the last value of sensor 1 is replaced by a 7.
```

**Constraints:**

- `sensor1.length == sensor2.length`
- `1 <= sensor1.length <= 100`
- `1 <= sensor1[i], sensor2[i] <= 100`

---

## Analysis

&emsp;&emsp;题目给出两个字符串`sensor1`和`sensor2`，说最多只有一个sensor是有问题的，需要我们判断出哪个sensor有问题。而有问题的sensor中间会漏掉一个元素，然后再末尾随机补上一个（而且补上的值和漏掉的值不一样）。

&emsp;&emsp;首先我们要把可能出现问题的点找出来，先同时开始遍历两个字符串，把没问题的部分先排除掉。

&emsp;&emsp;当碰到了`sensor1[i] != sensor2[i]`时，说明开始不一样了。情况有两种，第一种是有一个有问题，另一个正常；第二种是没办法通过序列来判断。先看第一种，如果是一个有问题，另一个正常的话，那么正常的跳过一个之后，应该和有问题的序列是一样的，比如Example1中`sensor1`是有问题的，因为跳过1后，`[3,4,5]`和`sensor2`的`[3, 4]`匹配了。

&emsp;&emsp;然后重点来看不能判断的情况，不能判断的情况就是两个字符串互相能够转换，比如`[2,3,2]`和`[3,2,3]`，因为都是刚好错开了一位，无法判断哪个是往后移了一位，所以我们就抓住这个关键去判断。

## Solution

&emsp;&emsp;判断移位的条件是`while (i + 1 < size && sensor1[i + 1] == sensor2[i] && sensor1[i] == sensor2[i + 1])`，如果是无法判断，那么`i`应该是等于`size - 1`，所以这个就是最后返回-1的条件。

------

## Code

```c++
class Solution {
public:
    int badSensor(vector<int>& sensor1, vector<int>& sensor2) {
        int i = 0, size = sensor1.size();
        while (i < size && sensor1[i] == sensor2[i]) {
            i++;
        }
        
        while (i + 1 < size && sensor1[i + 1] == sensor2[i] && sensor1[i] == sensor2[i + 1]) {
            // shifted to each other
            i++;
        }
        return i >= size - 1 ? -1 : sensor1[i] == sensor2[i + 1] ? 1: 2;
    }
};
```

------

## Summary

&emsp;&emsp;问题设计的算法不难，是简单的字符串遍历，但是要分析出无法判断的情况，还是需要一定的思考。这道题目的分享到这里，感谢你的支持！
