---
title: LeetCode解题报告（116）-- 497. Random Point in Non-overlapping Rectangles
tags:
  - LeetCode
mathjax: true
abbrlink: 5046
date: 2020-08-27 14:28:51
---

## Problem

Given a list of **non-overlapping** axis-aligned rectangles `rects`, write a function `pick` which randomly and uniformily picks an **integer point** in the space covered by the rectangles.

Note:

1. An **integer point** is a point that has integer coordinates. 
2. A point on the perimeter of a rectangle is **included** in the space covered by the rectangles. 
3. `i`th rectangle = `rects[i]` = `[x1,y1,x2,y2]`, where `[x1, y1]` are the integer coordinates of the bottom-left corner, and `[x2, y2]` are the integer coordinates of the top-right corner.
4. length and width of each rectangle does not exceed `2000`.
5. `1 <= rects.length <= 100`
6. `pick` return a point as an array of integer coordinates `[p_x, p_y]`
7. `pick` is called at most `10000` times.

<!-- more -->

**Example 1:**

```
Input: 
["Solution","pick","pick","pick"]
[[[[1,1,5,5]]],[],[],[]]
Output: 
[null,[4,1],[4,1],[3,3]]
```

**Example 2:**

```
Input: 
["Solution","pick","pick","pick","pick","pick"]
[[[[-2,-2,-1,-1],[1,0,3,0]]],[],[],[],[],[]]
Output: 
[null,[-1,-2],[2,0],[-2,-1],[3,0],[-2,-2]]
```

**Explanation of Input Syntax:**

The input is two lists: the subroutines called and their arguments. `Solution`'s constructor has one argument, the array of rectangles `rects`. `pick` has no arguments. Arguments are always wrapped with a list, even if there aren't any.

------

## Analysis

&emsp;&emsp;题目给出了一系列的长方形，然后要求在这些长方形里面随机选出一些点。点的坐标都是整数，我们如何去随机选出这些坐标值呢？大致上可以分为两个步骤。

&emsp;&emsp;第一步，先随机选择一个长方形。因为结果无论选择到什么点，它都会在某个长方形之内，所以我们先选择出长方形。

&emsp;&emsp;第二步，在上一步选好的长方形之内再选一个点。因为长方形的左下角和右上角坐标都给出，所以选择点并不难。

&emsp;&emsp;比较难的地方是在第一步，如何去选择长方形。我们可以用一个类似于累计概率的思路去做。每个长方形里面可以选的点实际上都是有限的，所以我们可以计算出每个长方形中可以选择点的个数。这个个数可以看作是一个池子，每新增一个长方形，这个池就扩大。随机的方法是：产生一个随机数，看看是这个随机数是落在新增的这个池，还是旧的，如果是前者，则选择当前的这个长方形。

------

## Solution

&emsp;&emsp;第一步产生随机数后，因为要判断是否落在新的池，所以要对总数先取余，确保落在总的池子中。同时需要特别注意，题目说长方形边界上的点也算在长方形内，所以计算数量的时候要+1。

------

## Code

```c++
class Solution {
public:
    Solution(vector<vector<int>>& rects) {
        this->rec = rects;
    }
    
    vector<int> pick() {
        int totalArea = 0;
        int size = this->rec.size();
        vector<int> selected;
        
        // choose a rectangle
        for (int i = 0; i < size; i++) {
            int current = (this->rec[i][3] - this->rec[i][1] + 1) * (this->rec[i][2] - this->rec[i][0] + 1);
            totalArea += current;
            if (rand() % totalArea < current) {
                selected = this->rec[i];
            }
        }
        
        // choose a point in the rectangle
        int y = rand() % (selected[3] - selected[1] + 1) + selected[1];
        int x = rand() % (selected[2] - selected[0] + 1) + selected[0];
        
        vector<int> result;
        result.push_back(x);
        result.push_back(y);
        return result;
    }
private:
    vector<vector<int>> rec;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(rects);
 * vector<int> param_1 = obj->pick();
 */
```

------

## Summary

&emsp;&emsp;这道是随机取样题目里面比较有难度的，但是本质的思路还是利用累计概率。在做题过程中还要特别考虑边界条件。这道题的分析到这里，谢谢！
