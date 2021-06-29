---
title: LeetCode解题报告（194）-- 735. Asteroid Collision
tags:
  - LeetCode
mathjax: true
abbrlink: 61644
date: 2020-10-21 17:12:58
---

## Problem

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

<!-- more -->

**Example 1:**

```
Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10.  The 5 and 10 never collide.
```

**Example 2:**

```
Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.
```

**Example 3:**

```
Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
```

**Example 4:**

```
Input: asteroids = [-2,-1,1,2]
Output: [-2,-1,1,2]
Explanation: The -2 and -1 are moving left, while the 1 and 2 are moving right. Asteroids moving the same direction never meet, so no asteroids will meet each other.
```

**Constraints:**

- `1 <= asteroids <= 104`
- `-1000 <= asteroids[i] <= 1000`
- `asteroids[i] != 0`

------

## Analysis

&emsp;&emsp;题目是以行星碰撞为背景的，相同运动方向不会碰撞，相反运动方向会碰撞。碰撞后大的保留，小的消失，如果两个一样则一同消失。特别注意碰撞后是不会抵消星球原本的大小的（即10和5碰撞之后，10还是10，5就消失）。

&emsp;&emsp;行星的运动方向只有正负，我们干脆先从一个方向（正方向）开始考虑。

- 对于正方向，我们会一直都插入到答案中。如果前面的行星是正方向，那么在后面插入一个正方向自然不会碰撞；如果前面的行星是负方向，两个行星是背道而驰，也不会有碰撞的问题。
- 对于负方向，就需要分多种情况考虑：
  - 答案为空：直接插入到答案中。前面没有任何行星，不存在碰撞的问题；
  - 答案最后一个为负方向：直接插入到答案中。两个都是负方向，所以也没有碰撞的可能；
  - 答案最后一个为正方向：这里就存在碰撞的情况，通过比较两个行星的绝对值，判断碰撞结果。如果正方向行星较大，碰撞后当前的负方向行星消失，不需要任何操作；如果负方向行星较大，碰撞后正方向行星消失，所以从答案中删掉最后一个行星，同时把下标往前置一位，下一次循环再次处理当前这个负方向的行星；如果两者一样，则同时消失，从答案中删掉最后一个行星，不需要再次处理当前这个负方向的行星。

------

## Solution

&emsp;&emsp;上面分析的思路非常清晰，只需要分类处理即可。

------

## Code

```c++
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        int size = asteroids.size();
        vector<int> result;
        for (int i = 0; i < size; i++) {
            if (asteroids[i] > 0) {
                result.push_back(asteroids[i]);
            } else if (result.empty() || result.back() < 0) {
                // empty or same direction
                result.push_back(asteroids[i]);
            } else if (result.back() <= -asteroids[i]) {
                if (result.back() < -asteroids[i]) {
                    i--;
                }
                result.pop_back();
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点是在于分析多种情况，coding方面没有难点。这类题目有一个特点就是，处理到当前的元素会对前面产生影响，所以要有一个往前退一步再次处理的逻辑。这道题目的分享到这里，谢谢！
