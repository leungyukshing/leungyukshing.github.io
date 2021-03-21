---
title: LeetCode解题报告（320)-- 478. Generate Random Point in a Circle
tags:
  - LeetCode
mathjax: true
date: 2021-03-20 18:16:24

---

## Problem

Given the radius and the position of the center of a circle, implement the function `randPoint` which generates a uniform random point inside the circle.

Implement the `Solution` class:

- `Solution(double radius, double x_center, double y_center)` initializes the object with the radius of the circle `radius` and the position of the center `(x_center, y_center)`.
- `randPoint()` returns a random point inside the circle. A point on the circumference of the circle is considered to be in the circle. The answer is returned as an array `[x, y]`.

<!-- more -->

**Example 1:**

```
Input
["Solution", "randPoint", "randPoint", "randPoint"]
[[1.0, 0.0, 0.0], [], [], []]
Output
[null, [-0.02493, -0.38077], [0.82314, 0.38945], [0.36572, 0.17248]]

Explanation
Solution solution = new Solution(1.0, 0.0, 0.0);
solution.randPoint(); // return [-0.02493, -0.38077]
solution.randPoint(); // return [0.82314, 0.38945]
solution.randPoint(); // return [0.36572, 0.17248]
```

**Constraints:**

- `0 < radius <= 108`
- `-107 <= x_center, y_center <= 107`
- At most `3 * 104` calls will be made to `randPoint`.

------

## Analysis

&emsp;&emsp;题目通过圆心和半径的形式给定一个圆，要求随机生成一个在圆中的点。我们先来看怎么去生成在圆里面的坐标。先看圆的坐标方程是：$(x - a)^2 + (y - b)^2 = r^2$，但是在这里去生成一对符合要求的`(x,y)`很复杂，这个时候就需要用到一些数学知识了。我们转化成极坐标来计算，此时$x = a + \rho \cos\theta$，$y = b + \rho \sin \theta$，此时要考虑的两个变量是$\rho$和$\theta$。

&emsp;&emsp;对于 $\rho$来说，可以直接使用随机数生成，只需要生成一个`[0,1]`之间的随机数，乘上$2\pi$即可；但是对于$\rho$来说就不可以了。因为圆的极坐标方程是$(\rho \cos \theta)^2 + (\rho\sin\theta)^2$，如果我们直接随机出一个$\rho$那么平方之后就不是等概率了，因为两个小于1的数字相乘会更加靠近0。所以我们的做法应该是随机后取个平方根再乘上半径，这样就是等概率了。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    Solution(double radius, double x_center, double y_center) {
        this->radius = radius;
        this->x_center = x_center;
        this->y_center = y_center;
    }
    
    vector<double> randPoint() {
        double theta = 2 * M_PI * ((double)rand() / RAND_MAX);
        double r = sqrt((double)rand() / RAND_MAX) * this->radius;
        return {this->x_center + r * cos(theta), this->y_center + r * sin(theta)};
    }
private:
    double radius, x_center, y_center;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(radius, x_center, y_center);
 * vector<double> param_1 = obj->randPoint();
 */
```

------

## Summary

&emsp;&emsp;这道题是一道偏向于数据和概率的题目，在解决随机生成问题的时候一定要注意平方运算带来的不等概率的问题。这道题目的分享到这里，感谢你的支持！
