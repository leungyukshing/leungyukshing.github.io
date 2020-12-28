---
title: LeetCode解题报告（253）-- 754. Reach a Number
tags:
  - LeetCode
mathjax: true
date: 2020-12-28 20:15:24

---

## Problem

You are standing at position `0` on an infinite number line. There is a goal at position `target`.

On each move, you can either go left or right. During the *n*-th move (starting from 1), you take *n* steps.

Return the minimum number of steps required to reach the destination.

<!-- more -->

**Example 1:**

```
You are standing at position 0 on an infinite number line. There is a goal at position target.

On each move, you can either go left or right. During the n-th move (starting from 1), you take n steps.

Return the minimum number of steps required to reach the destination.
```

**Example 2:**

```
Input: target = 2
Output: 3
Explanation:
On the first move we step from 0 to 1.
On the second move we step  from 1 to -1.
On the third move we step from -1 to 2.
```

**Note:**

- `target` will be a non-zero integer in the range `[-10^9, 10^9]`.

------

## Analysis

&emsp;&emsp;这道题是说在一个数轴上，从0点出发，第`n`步的步长为`n`，可以向左或右行走，问多少步能够到达给定的数字。我们来看下面两种情况：

1. 一直走刚好就走到了，这种情况是最好的，比如1、3、6、10这样；
2. 不能刚好走到，需要往反方向走一下，再正向走的。

&emsp;&emsp;当我们知道`target`后，第一种情况是非常好计算的，而第二种就比较复杂。我们对第二种情况再做细分，假设我们先一直走到比`target`大的数`total`，走了`n`步，然后二者作差`diff = total - target`，这个时候如果`diff`是偶数就比较好办。因为在走的过程中，第`diff`步的步长就是`diff`，当走第`diff / 2`步的时候，只需要往反方向走，然后再正方向走`diff / 2`，这样就相当于抵消了`diff`的长度，这样就能刚好到达`target`的位置了，所以这种情况下直接就是走了`n`步，其中有两步是互相抵消的。

&emsp;&emsp;如果`diff`是奇数又要如何处理呢？实际上我们还是借助上面的思路，只不过需要多走一步`n + 1`。再走一步`n + 1`之后作差，假设是`diff2`，如果`n + 1`为奇数的话，又因为`diff`是奇数，所以`diff2`一定是偶数，回到上面的情况，所以答案就是`n + 1`；如果`n + 1`为偶数的话，就需要往后再走一步`n + 2`，才能让这个差的奇数变成偶数，所以答案就是`n + 2`。

------

## Solution

&emsp;&emsp;根据上面的分析，关键是首先定位到`n`，要先找到第一个比`target`大的，所以可以直接用平方根公式，并使用`ceil()`**往上取整**。再根据上面分析的情况做判断。

------

## Code

```c++
class Solution {
public:
    int reachNumber(int target) {
        target = abs(target);
        long n = ceil((-1.0 + sqrt(1 + 8.0 * target)) / 2);
        long total = n * (n + 1) / 2;
        if (total == target) {
            return n;
        }
        
        long diff = total - target;
        if (diff % 2 == 0) {
            return n;
        } else {
            return n + ((n & 1) ? 2: 1);
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目运用到了一些数学的知识，属于偏数学分析的编程题，coding方面没有过多的难点。这类题目比较考察数学思维和敏感度，需要多总结。这道题目的分享到这里，谢谢您的支持！
