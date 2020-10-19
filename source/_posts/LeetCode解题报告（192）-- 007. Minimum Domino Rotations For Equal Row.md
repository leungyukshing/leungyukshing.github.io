---
title: LeetCode解题报告（192）-- 007. Minimum Domino Rotations For Equal Row
tags:
  - LeetCode
mathjax: true
date: 2020-10-19 23:55:18


---

## Problem

In a row of dominoes, `A[i]` and `B[i]` represent the top and bottom halves of the `ith` domino.  (A domino is a tile with two numbers from 1 to 6 - one on each half of the tile.)

We may rotate the `ith` domino, so that `A[i]` and `B[i]` swap values.

Return the minimum number of rotations so that all the values in `A` are the same, or all the values in `B` are the same.

If it cannot be done, return `-1`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/03/08/domino.png)

```
Input: A = [2,1,2,4,2,2], B = [5,2,6,2,3,2]
Output: 2
Explanation: 
The first figure represents the dominoes as given by A and B: before we do any rotations.
If we rotate the second and fourth dominoes, we can make every value in the top row equal to 2, as indicated by the second figure.
```

**Example 2:**

```
Input: A = [3,5,1,2,3], B = [3,6,3,3,4]
Output: -1
Explanation: 
In this case, it is not possible to rotate the dominoes to make one row of values equal.
```

**Constraints:**

- `2 <= A.length == B.length <= 2 * 104`
- `1 <= A[i], B[i] <= 6`

------

## Analysis

&emsp;&emsp;题目叙述的有点复杂了，这里我简化一下，题目给出两个数组`A`和`B`，每个位置可以进行交换，问经过多少次交换，可以使得其中一个数组中的元素都是同一个值。

&emsp;&emsp;之前说过，这类操作类型的题目，基本上不可能要一次一次操作做完的，大概率会超时，所以必须是巧解。这道题目顺着想会比较困难，因为我们不知道要交换哪几个位置，甚至不知道要的值是什么，所以非常难进行判断。这个时候不妨从结果倒推。如果两个数组能经过若干次交换，使得最后有一个数组里面的元素都一样的话，那么这个数组中的每一个元素，要么来自于`A`，要么来自于`B`。因为元素都一样（假设值都是`target`），所以开头第一个值（其实任意一个值都是，为了方便起见选第一个）也是这个值，它也是要么来自于`A`，要么来自于`B`。

&emsp;&emsp;也就是说，在有答案的情况下，最终数组的第一个值要么是`A`的第一个值，要么是`B`的第一个值。这样我们就找到了判断的标准，只需要分两种情况来看。

&emsp;&emsp;解决了用什么值来判断的问题，然后就解决交换的问题。因为最后的数组是`A`和`B`两个数组共同贡献的，所以就记录一下在哪些位置上和`A`数组一样，在哪些位置上和`B`数组一样，取较小的一个就可以了。

------

## Solution

&emsp;&emsp;分别用`A`和`B`的第一个元素作为`target`，各自判断一遍是否满足题目要求，如果有满足的就选交换少；如果都不满足的话就是无解，返回-1。

------

## Code

```c++
class Solution {
public:
    int minDominoRotations(vector<int>& A, vector<int>& B) {
        int a = 0, b = 0, size = A.size();
        for (int i = 0, a = 0, b = 0; i < size && (A[i] == A[0] || B[i] == A[0]); i++) {
            if (A[i] == A[0]) {
                a++;
            }
            if (B[i] == A[0]) {
                b++;
            }
            if (i == size - 1) {
                return min(size - a, size - b);
            }
        }
        
        for (int i = 0, a = 0, b = 0; i < size && (A[i] == B[0] || B[i] == B[0]); i++) {
            if (A[i] == B[0]) {
                a++;
            }
            if (B[i] == B[0]) {
                b++;
            }
            if (i == size - 1) {
                return min(size - a, size - b);
            }
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道数组类的题目有点意思，在正向思考受阻的情况下，不妨从答案倒推，会对做题有很大的帮助。这道题目的分享到这里，谢谢！
