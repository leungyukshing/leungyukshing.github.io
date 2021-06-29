---
title: LeetCode解题报告（344) -- 667. Beautiful Arrangement II
tags:
  - LeetCode
mathjax: true
abbrlink: 48901
date: 2021-04-18 21:06:35
---

## Problem

Given two integers `n` and `k`, construct a list `answer` that contains `n` different positive integers ranging from `1` to `n` and obeys the following requirement:

- Suppose this list is `answer = [a1, a2, a3, ... , an]`, then the list `[|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|]` has exactly `k` distinct integers.

Return *the list* `answer`. If there multiple valid answers, return **any of them**.

<!-- more -->

**Example 1:**

```
Input: n = 3, k = 1
Output: [1,2,3]
Explanation: The [1,2,3] has three different positive integers ranging from 1 to 3, and the [1,1] has exactly 1 distinct integer: 1
```

**Example 2:**

```
Input: n = 3, k = 2
Output: [1,3,2]
Explanation: The [1,3,2] has three different positive integers ranging from 1 to 3, and the [2,1] has exactly 2 distinct integers: 1 and 2.
```

**Constraints:**

- `1 <= k < n <= 104`

------

## Analysis

&emsp;&emsp;题目给出数字`n`和`k`，要求先构建一个数组，里面的值就是`[1, n]`的每一个数字，要求对这个数组中数字的顺序进行调整，使得每相邻的两数之差的绝对值有`k`个不同的值。文字不太好理解，直接看例子。以`n = 8`为例，最原始的状态就是`[1,2,3,4,5,6,7,8]`，在这种情况下每相邻两数之差的绝对值都是1。题目要求的就是要让这些绝对值有`k`个不同的值。

&emsp;&emsp;先来看最极端的情况，最多能够有多少个不一样。为了制造最大的差，选取头尾两个数字`1`和`8`，依次地选取`2` 和`7`、`3`和`6`、`4`和`5`。重组后的数组是`[1,8,2,7,3,6,4,5]`，它们的差的绝对值分别是7、6、5、4、3、2、1，这样就能制造出最多不同的绝对值。通过这个例子，我们知道怎么样去构造出不同的绝对值，现在的问题来到如何构造出指定数量的绝对值？

&emsp;&emsp;其实非常简单，按需构造，剩下的按照顺序排列即可。原因是，按照上面的构造方式，每交换两个，能够得到两个不同的绝对值，然后把剩下的按照顺序排列，他们的绝对值之差都是1，这样就能够得到指定的数量了。

------

## Solution

&emsp;&emsp;使用左右两个指针取数字，因为一共要`k`个不同，要预留后面的默认都是1，所以前面要构造`k - 1`个不同的绝对值。选数字也是有技巧的，这里要考虑前面搭配完之后，和后面有序序列中间的那个差。这里我默认后面都是从小到大排序，所以前面先选左还是先选右就需要考虑。因为当`k = 1`的时候，是使用默认的1，所以当`k`为奇数时，选取左边的元素；当`k`为偶数时，选取右边的元素。

------

## Code

```c++
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        int left = 1, right = n;
        vector<int> result;
        while (k > 1 && left < right) {
            if (k % 2 == 1) {
                result.push_back(left);
                left++;
            } else {
                result.push_back(right);
                right--;
            }
            k--;
        }
        cout << left << " " << right << endl;
        for (int i = left; i <= right; i++) {
            result.push_back(i);
        }
        
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是数组中构造类型题目中比较复杂的，首先需要找出构造出不同差的绝对值的方法，然后还需要利用奇偶规律决定先选取哪边。这道题目的分享到这里，感谢你的支持！
