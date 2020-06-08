---
title: LeetCode解题报告（84）-- 528. Random Pick with Weight
tags:
  - LeetCode
date: 2020-06-08 15:03:02
mathjax: true

---

## Problem

Given an array `w` of positive integers, where `w[i]` describes the weight of index `i`, write a function `pickIndex` which randomly picks an index in proportion to its weight.

Note:

1. `1 <= w.length <= 10000`
2. `1 <= w[i] <= 10^5`
3. `pickIndex` will be called at most `10000` times.

<!-- more -->

**Example 1:**

```
Input: 
["Solution","pickIndex"]
[[[1]],[]]
Output: [null,0]
```

**Example 2:**

```
Input: 
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
Output: [null,0,1,1,1,0]
```

**Explanation of Input Syntax:**

The input is two lists: the subroutines called and their arguments. `Solution`'s constructor has one argument, the array `w`. `pickIndex` has no arguments. Arguments are always wrapped with a list, even if there aren't any.

------

## Analysis

&emsp;&emsp;题目的意思其实很简单，就是给出一个数组，里面给出了每个下标对应的权重。然后再按照这个权重随机选取下标，难点在于**不是等概率**的。

&emsp;&emsp;解决的方法也很简单，我们采用类似**累积分布函数**的方法，将每个下标所占的概率累加起来。如题目例2，两个下标一共占4份，其中0占1份，1占3份。计算累积分布，得到$[1, 4]$。然后我们生成一个随机数，取余后，使得结果在$[0, 3]$这个范围内，然后再根据累积分布函数找到对应的下标，当取余后为0，则选择0号，其他结果则选择1。

------

## Solution

&emsp;&emsp;生成随机数使用`rand()`即可，记得要取余。

------

## Code

```c++
class Solution {
public:
    Solution(vector<int>& w) {
        int total = 0;
        for (int i = 0; i < w.size(); i++) {
            total += w[i];
            this->cdf.push_back(total);
        }
        this->max = total;
    }
    
    int pickIndex() {
        int idx = rand() % this->max;
        // cout << idx << endl;
        for (int i = 0; i < this->cdf.size(); i++) {
            if (idx < this->cdf[i]) {
                return i;
            }
        }
        return -1;
    }
private:
    int max;
    vector<int> cdf;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```

------

## Summary

 &emsp;&emsp;这道题目本身难度不大，但是是一种很经典的负采样方法的基础——UnigramTable。希望这篇博客能够帮助到大家，谢谢！
