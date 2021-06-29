---
title: 计算几何之判断某个点是否在多边形中
mathjax: true
abbrlink: 48365
date: 2020-06-23 11:40:21
tags:
---

## Problem

**Link: **http://acm.hdu.edu.cn/showproblem.php?pid=1756

&emsp;&emsp;借用hdu这道算法题，今天来和大家探讨计算几何中一个很经典的问题——给出一个多边形和一个点，如何判断这个点是否在多边形里面？

<!-- more -->

**Example:**

```
Input:
4
10 10
20 10
20 5
10 5
2
15 8
25 8
Output:
1
0
```

------

## Analysis

&emsp;&emsp;面对这个问题，一下子可以想到很多的方法，比如说对比一下点和顶点的位置之类的，但是还是有很多边缘的case覆盖不到，所以这里我也没有从零思考，而是直接看计算几何的资料。计算几何中给出两个比较出名的方法：**射线法**、**角度法**。这篇博客我只介绍射线法。

&emsp;&emsp;射线法比较好理解，其基本原理是：**从待验证的点引出一条射线，计算这条射线与多边形边界的交点个数。**当然还有很多special case要考虑，比如一下几种情况：

- 射线与多边形某条边重叠
- 射线经过了多边形的顶点

&emsp;&emsp;实际的操作是，从待验证的点水平向右引出射线，遵循如下规则：

- 这个点本身就在多边形的某条边上，那么这个点就属于多边形内部；
- 如果射线经过了多边形的顶点，若这个多边形的顶点在边上的纵坐标较大，则计数，否则忽略；
- 多边形的水平边忽略。

------

## Solution

&emsp;&emsp;相信经过上面的分析，大家在纸上应该可以体会到三条规则的使用，但是到了coding层面上，难度稍微提高了。我们要判断几个东西：

- 点`Q`是否在线段`AB`上
- 点`Q`相对于线段`AB`在什么位置

 &emsp;&emsp;对于第一个来说，我们采用的是点乘和叉乘。点乘小于等于0可以保证点`Q`不在线段`AB`的延长线上。叉乘为0保证了点`Q`在直线`AB`上。两个结合使用就可以确定点`Q`是否在线段`AB`上了。

&emsp;&emsp;第二点的解决会比较巧妙，首先判断**射线不经过纵坐标最小的多边形顶点**，并且点`Q`是在线段`AB`的纵坐标范围内的，判断条件是$dcmp(y_a - y_q) > 0 != dcmp(y_b - y_q) > 0$。然后判断射线与线段的交点是否在点`Q`的右边。方法就是建立`AB`的直线方程，然后当纵坐标相等的时候，求出对应的横坐标，再与点`Q`的横坐标比较就可以了。假设点`Q`坐标为$(x_q, y_q)$，点`A`坐标为$(x_a, y_a)$，点`B`坐标为$(x_b, y_b)$，数学推导如下：

直线AB的方程为
$$
\frac{y - y_b}{y_a - y_b} = \frac{x - x_b}{x_a - x_b} \\
$$
代入点`Q`的纵坐标，得出交点的横坐标$x_p$：
$$
x_p = \frac{(y - y_b)(x_a - x_b)}{(y_a - y_b)} + x_b
$$
然后比较$x_p$和$x_q$，如果$x_q \lt x_p$，则说明有一个交点。

&emsp;&emsp; 通过对多边形所有的边都进行判断，使用一个布尔变量`flag`来记录交点的奇偶，最后得到结果。

------

## Code

```c++
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

const double eps = 1e-6;

struct Point {
	double x, y;

	Point(double x = 0, double y = 0): x(x), y(y) {}

	Point operator + (const Point &b) const {
		return Point(x + b.x, y + b.y);
	}

	Point operator - (const Point &b) const {
		return Point(x - b.x, y - b.y);
	}

	double operator * (const Point &b) const {
		return x * b.x + y * b.y;
	}

	double operator ^ (const Point &b) const {
		return x * b.y - b.x * y;
	}
};

typedef Point Vector;

int dcmp(double x) {
	if (fabs(x) < eps) {
		return 0;
	}
	else {
		return x < 0? -1: 1;
	}
}

bool OnSegment(Point p1, Point p2, Point q) {
	// cross product is 0 -> on the same line
	// dot product <= 0 -> on the segment
	return dcmp((p1 - q) ^ (p2 - q)) == 0 && dcmp((p1 - q) * (p2 - q)) <= 0;
}

bool InPolygon(vector<Point>& polygon, Point q) {
	bool flag = false;

	Point p1, p2;
	int n = polygon.size();
	for (int i = 0, j = n - 1; i < n; j=i++) {
		p1 = polygon[i];
		p2 = polygon[j];

		if (OnSegment(p1, p2, q)) {
			return true;
		}
		// p1, p2 line: (y - y1) / (y1 - y2) = (x - x2) / (x1 - x2)
		// intersaction point of line p1p2 and q: xa = (y - y2) * (x1 - x2) / (y1 - y2) + x2
		// q is on the left of the intersaction point: x - xa < 0
		if ((dcmp(p1.y - q.y) > 0 != dcmp(p2.y - q.y) > 0) && dcmp(q.x - ((q.y - p2.y) * (p1.x - p2.x) / (p1.y - p2.y) + p2.x)) < 0) {
			flag = !flag;
		}
	}
	return flag;
}

int main() {
	int n = 0;
	int x = 0, y = 0;
	vector<Point> polygon;
	cin >> n;
	while(n--) {
		cin >> x >> y;
		Point p(x, y);
		polygon.push_back(p);
	}
	cin >> n;
	while (n--) {
		cin >> x >> y;
		Point test(x, y);
		cout << InPolygon(polygon, test) << endl;
	}
	
	return 0;
}
```

------

## Summary

 &emsp;&emsp;这是我第一次比较深入地探讨计算几何的内容，这一类题目还是需要先在纸上画出来，分类讨论好之后，再进行coding。在判断的时候多数情况下都会使用坐标法，但是对于线段的判断就需要额外的条件，这篇博客提到的点乘和叉乘的方法是比较常用的，可以积累下来。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
