---
title: LeetCode解题报告（135）-- 1291. Sequential Digits
tags:
  - LeetCode
mathjax: true
date: 2020-09-19 22:09:11


---

## Problem

An integer has *sequential digits* if and only if each digit in the number is one more than the previous digit.

Return a **sorted** list of all the integers in the range `[low, high]` inclusive that have sequential digits.

<!-- more -->

**Example 1:**

```
Input: low = 100, high = 300
Output: [123,234]
```

**Example 2:**

```
Input: low = 1000, high = 13000
Output: [1234,2345,3456,4567,5678,6789,12345]
```

**Constraints:**

- `10 <= low <= high <= 10^9`

------

## Analysis

&emsp;&emsp;这道题的意思很简单，找出某个区间内的sequential number。sequential number就是从高到低，每一位都比前面大1的数。在某个指定的区间内去遍历，这些计算都是有限的，所以穷搜是一种很直接的做法。

&emsp;&emsp;但是作为题解，当然要选择一种更加高效的方法。我们可以从sequential number的特点入手，以example2为例，如果我们有了1234，那么12345实际上是不需要从头开始构造的，我们只需要在1234的基础上往后增加，在增加后判断新的数12345是否满足题目的要求就可以了。所以思路就是先按相同位数、不同开头的遍历，然后再提升位数。所以`n+1`位的数是从`n`位数构造而来的，这个思路就有点像BFS了。

------

## Solution

&emsp;&emsp;用队列存储数据，这样就能保证先遍历完`n `位数，再遍历`n+1`位数。因为是按顺序的，所以当某个数超过范围大小，遍历就可以直接结束了。

&emsp;&emsp;还需要注意的是当最后一位数字是9的时候，就不能继续往后增加数字了。

------

## Code

```c++
class Solution {
public:
    vector<int> sequentialDigits(int low, int high) {
        queue<int> q;
        for (int i = 1; i < 10; i++) {
            q.push(i);
        }
        vector<int> result;
        while (!q.empty()) {
            int num = q.front();
            q.pop();
            
            if (num > high) {
                break;
            }
            if (num >= low && num <= high) {
                result.push_back(num);
            }
            if (num % 10 == 9) {
                // can not generate sequential nubmer any more
                continue;
            }
            q.push(num * 10 + num % 10 + 1); // 1 => 12
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目要pass并不难，但是仔细琢磨一下还是有很多值得思考的点，使用BFS去解决数字相关的问题也是一种思路，因为数字中的位数和二叉树中的层数是非常类似的概念。这道题的分析到这里，谢谢！
