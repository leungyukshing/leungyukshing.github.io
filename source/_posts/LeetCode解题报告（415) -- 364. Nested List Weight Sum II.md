---
title: LeetCode解题报告（415) -- 364. Nested List Weight Sum II
mathjax: true
date: 2021-12-12 15:18:41
---

## Problem

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists.

The **depth** of an integer is the number of lists that it is inside of. For example, the nested list `[1,[2,2],[[3],2],1]` has each integer's value set to its **depth**. Let `maxDepth` be the **maximum depth** of any integer.

The **weight** of an integer is `maxDepth - (the depth of the integer) + 1`.

Return *the sum of each integer in* `nestedList` *multiplied by its **weight***.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/03/27/nestedlistweightsumiiex1.png)

```
Input: nestedList = [[1,1],2,[1,1]]
Output: 8
Explanation: Four 1's with a weight of 1, one 2 with a weight of 2.
1*1 + 1*1 + 2*2 + 1*1 + 1*1 = 8
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/03/27/nestedlistweightsumiiex2.png)

```
Input: nestedList = [1,[4,[6]]]
Output: 17
Explanation: One 1 at depth 3, one 4 at depth 2, and one 6 at depth 1.
1*3 + 4*2 + 6*1 = 17
```

**Constraints:**

- `1 <= nestedList.length <= 50`
- The values of the integers in the nested list is in the range `[-100, 100]`.
- The maximum **depth** of any integer is less than or equal to `50`.

------

## Analysis

&emsp;&emsp;题目给出一个nestedList，要求用每个元素元素的depth计算出对应的weights，然后计算出所有元素的加权和。先看正常的思路，因为我们要计算出一个`maxDepth`，所以我们要所有的元素都遍历一遍得到这个`maxDepth`。然后再遍历一次，计算出每个元素的`weight`，最后求和。遍历的方法可以采用DFS或者BFS，总之就需要2 pass。

&emsp;&emsp;那么有没有一种可以1 pass的方法呢？这里我们对weight计算做一个变换。变换如下：
$$
result = \sum_{i = 1}^n(maxDepth - depth + 1) \times a_i \\
= maxDepth \times \sum_{i = 1}^n - depth \times \sum_{i = 1}^na_i + \sum_{i = 1}^na_i \\
= (maxDepth + 1)\sum_{i = 1}^na_i - \sum_{i = 1}^ndepth \times a_i
$$
&emsp;&emsp;经过上述的变换，我们就把`maxDepth`拆出来了，于是我们一次遍历就可以实现。遍历过程中，我们可以求出所有元素的和，同时还求出每个元素和自己深度的乘积的和，并且记录一个最大的深度，最后就能得到答案。

&emsp;&emsp;这里再提一下BFS。首先把传入的`nestedList`中的每个元素都放入到队列中。然后从队列中拿出元素，判断是数字还是一个`NestedInteger`，如果是数字就按照上面的公式进行计算，如果还是一个List，就对里面的每个元素继续加到队列中。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        queue<NestedInteger> q;
        for (auto ele: nestedList) {
            q.push(ele);
        }
        
        int sumOfElement = 0;
        int productOfElement = 0;
        int depth = 1;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto cur = q.front();
                q.pop();
                
                if (cur.isInteger()) {
                    sumOfElement += cur.getInteger();
                    productOfElement += cur.getInteger() * depth;
                } else {
                    for (auto ele: cur.getList()) {
                        q.push(ele);
                    }
                }
            }
            depth++;
        }
        
        return depth * sumOfElement - productOfElement;
    }
};
```

------

## Summary

&emsp;&emsp;对于嵌套数据结构来说，BFS的遍历是一种比较常规的遍历手段，这个是比较简单的。这道题目的难点在于如果做到1 pass。对于计算深度权重这类的题目，不妨把数学公式列出来，然后进行化简计算。目的是把某些值（如这道题目中的`maxDepth`）抽取出来，独立于每个元素权重的计算。这道题目的分享到这里，感谢你的支持！
