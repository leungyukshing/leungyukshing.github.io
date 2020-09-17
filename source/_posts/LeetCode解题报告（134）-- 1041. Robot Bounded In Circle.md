---
title: LeetCode解题报告（134）-- 1041. Robot Bounded In Circle
tags:
  - LeetCode
mathjax: true
date: 2020-09-17 16:20:28

---

## Problem

On an infinite plane, a robot initially stands at `(0, 0)` and faces north.  The robot can receive one of three instructions:

- `"G"`: go straight 1 unit;
- `"L"`: turn 90 degrees to the left;
- `"R"`: turn 90 degress to the right.

The robot performs the `instructions` given in order, and repeats them forever.

Return `true` if and only if there exists a circle in the plane such that the robot never leaves the circle.

<!-- more -->

**Example 1:**

```
Input: "GGLLGG"
Output: true
Explanation: 
The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.
```

**Example 2:**

```
Input: "GG"
Output: false
Explanation: 
The robot moves north indefinitely.
```

**Example 3:**

```
Input: "GL"
Output: true
Explanation: 
The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...
```

**Note:**

1. `1 <= instructions.length <= 100`
2. `instructions[i]` is in `{'G', 'L', 'R'}`

------

## Analysis

&emsp;&emsp;这道题是非常典型的步骤题或者叫流程题，题目的特征是题目给出了一系列的操作，要求最终的结果或者判断最后能否到达某个状态。这道题目就是给出了一系列的操作，这些操作是可以重复的，要求判断经过这一系列操作后，这个robot的活动范围在一个有限的圆圈内。

&emsp;&emsp;显然去找这个圆圈是比较不实际的，因为他有可能很大，也不知道圆心在哪，所以我们要换个角度思考问题。同时这类题目如果是按着给出的操作步骤一直操作下去的话，很容易会超时，因此要巧解。先看题目这个有限的圆圈的含义，实际上如果经历若干步之后，robot都不超出某个圈，那就说明它能回到了起点。因为如果无论如何都不能回到起点的话，这个robot就会往着某个方向不断偏，也就没有一个circle能够框住了。

&emsp;&emsp;然后我们看题目给出的有限步操作，经过这些有限步的操作，最终会有三个结果：

1. robot回到原点；
2. robot没有回到原点，面朝北；
3. robot没有回到原点，面朝除了北之外的其他方向。

&emsp;&emsp;第一种情况是能直接回到原点的，所以是符合题目条件的；第三种情况实际上也是可以回到原点的，因为他有一个偏离，经过无限次操作后，这个偏离总会抵消掉；第二种情况则不能了，因为第一次做完题目给的操作之后，它不在原点，而且也是面朝北的，所以和初始状态相比它只往北方向有位移，通过若干次操作后它只会越来越远。

&emsp;&emsp;所以到这里思路就比较清晰了，我们只需要把题给出的有限步操作做完就能够判断了，而不需要一直走。

------

## Solution

&emsp;&emsp;为了方便起见有数字表示方向，然后计算在横、纵两个方向上的偏移量即可。

------

## Code

```c++
class Solution {
public:
    bool isRobotBounded(string instructions) {
        // North:0, East:1, South:2, West:3
        int currentDirection = 0;
        int delta_x = 0;
        int delta_y = 0;
        
        int size = instructions.size();
        for (int i = 0; i < size; i++) {
            if (instructions[i] == 'G') {
                if (currentDirection == 0) {
                    delta_y++;
                } else if (currentDirection == 1) {
                    delta_x++;
                } else if (currentDirection == 2) {
                    delta_y--;
                } else {
                    delta_x--;
                }
            } else if (instructions[i] == 'L') {
                if (currentDirection == 0) {
                    currentDirection = 3;
                } else {
                    currentDirection--;
                }
            } else {
                currentDirection = (currentDirection + 1) % 4;
            }
        }
        if ((delta_x == 0 && delta_y == 0) || currentDirection != 0) {
            return true;
        } else {
            return false;
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点在于前面的思考，后面的coding部分是比较简单的。关键的点在于如何通过有限步的操作去判断最后的结果。这道题的分析到这里，谢谢！
