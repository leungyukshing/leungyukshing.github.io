---
title: 网易面试算法题分享
tags:
  - 面试总结
mathjax: true
abbrlink: 25544
date: 2019-08-17 18:28:48
---

## Intro

&emsp;&emsp;碰巧帮一个朋友看了一道网易今年的校招面试算法题，觉得挺有意思的，这里分享一下自己的做法，如果觉得有问题的欢迎指出！

<!-- more -->

---

## Problem

&emsp;&emsp;小易有一个长度为$n$的数字数组$a\_1, a\_2, \cdots, a\_n$，问：是否能用这$n$个数字构成一个环（首尾相连），使得环中的每一个数字都小于它相邻的两个数字的和（每个数字都必须使用并且每个数字只能使用一次）

**输入描述**

第一行包含一个整数$t(1 \le t \le 10)$，表示测试用例的组数

每个测试用例的输入如下：

第一行一个整数$n$，表示数字的个数；

第二行$n$个整数$a\_1, a\_2, \cdots, a\_n$，每两个整数之间用一个空格分隔。

**输入数据保证**

$3 \le n \le 10^5$

$1 \le a_i \le 10^9$

**输出描述**

输出应该包含$t$行，对于每组用例，若能输出“YES“，否则输出”NO“。

**Example1:**

```
Input:
1
5
17 6 17 11 17

Output:
YES
```

**Example2:**

```
Input:
1
3
1 2 4

Output:
NO
```

---

## Analyze

&emsp;&emsp;这是一道有关于数组的题目，针对的是数字类型的数据。首先这道题目问的是一个是或不是的答案，并不是要我们输出所有可能的结果，一般这类题目并不需要我们把所有的情况都列举出来，所以穷举的方法大概率会超时。

&emsp;&emsp;确认好方向后，我们就来根据题目的要求进行思考。这道题目的要求就是，每个元素的左右两边元素之和比它本身大。这个要求一眼看上去很复杂，因为可能性有很多。我们不妨先换个思路，先看看题目给出来的case的特点。我们把case1的数字构成一个环，会发现顺序是6->11->17->17->17。当然这个顺序是一个环，但是如果你从最小的元素开始看，就会发现者实际上是一个**递增的有序数组**。

&emsp;&emsp;这个**有序**是一个切入点，因为如果这个条件是可以基于有序来判断的，那么我们的工作就变得非常简单。对于一个有序递增数组的某一个元素$a_i$，它的后一个元素$a_{i+1}$肯定比它大，因此$a\_{i+1} + a\_{i-1} \gt a\_i$必然成立。而题目要求的是一个环，所以说在有序数组的基础上，我们只需要判断数组的首尾能否构成这个环就可以了。因为数组的最后一个元素$a_n$是整个数组中最大的元素，$a\_{n-1} + a\_1$很有可能比它小，所以我们只需要判断$a\_{n-1}+a\_1$和$a\_n$的关系即可，如果满足题目要求则输出YES，否则输出NO。

---

## Code

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
  vector<int> arr;
  int caseNum;
  cin >> caseNum;

  while (caseNum--) {
    int n;
    cin >> n;
    int temp;
    for (int i = 0; i < n; i++) {
      cin >> temp;
      arr.push_back(temp);
    }

    sort(arr.begin(), arr.end());

    if (arr[0] + arr[n - 2] > arr[n - 1]) {
      cout << "YES" << endl;
    }
    else {
      cout << "NO" << endl;
    }
  }
  return 0;
}
```

---

## Summary

&emsp;&emsp;要在面试时快速地解题不但需要很强的coding能力，而且还需要对不同题型的题目有一定的敏锐程度。以这题为例，敏锐程度体现在，对于数组类型的题目，我们优先考虑排序的做法。又比如说其他的一些求最小值最大值的题目，我们是可以优先考虑贪心算法的。本道题的分析到这里，希望能帮助到大家，谢谢！
