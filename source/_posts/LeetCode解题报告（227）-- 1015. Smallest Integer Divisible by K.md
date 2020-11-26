---
title: LeetCode解题报告（227）-- 1015. Smallest Integer Divisible by K
tags:
  - LeetCode
mathjax: true
date: 2020-11-26 02:09:11

---

## Problem

Given a positive integer `K`, you need to find the **length** of the **smallest** positive integer `N` such that `N` is divisible by `K`, and `N` only contains the digit `1`.

Return *the **length** of* `N`. If there is no such `N`, return -1.

**Note:** `N` may not fit in a 64-bit signed integer.

<!-- more -->

**Example 1:**

```
Input: K = 1
Output: 1
Explanation: The smallest answer is N = 1, which has length 1.
```

**Example 2:**

```
Input: K = 2
Output: -1
Explanation: There is no such positive integer N divisible by 2.
```

**Example 3:**

```
Input: K = 3
Output: 3
Explanation: The smallest answer is N = 111, which has length 3.
```

**Constraints:**

- `1 <= K <= 10^5`

------

## Analysis

&emsp;&emsp;这道题目可以归为搜索类题目，这类题目往往给定要求，要在一定的范围内找到所有满足要求的数（或者问是否存在），当然这里要求的是数字的长度。这类题目的解法大多有两种：一是构造；二是搜索。比方说回文数的题目，很多都是通过构造来解决的。这道题目的要求是整除`K`并且数字是由1组成的，最直接的思路就是暴力求解，实际上在这个范围内仅由1组成的数是有限的，所以通过暴力搜索是可以解决问题的，但是既然我们做题就肯定要找一个更好的方法。

&emsp;&emsp;暴力搜索的问题在于搜索的范围太大了，我们不知道搜到什么才停止。如果说我们能够在较少的步骤下就知道能否整除，我们搜索的范围就会大大减少。这里就涉及到一些数学的推理了。我们要找的是一个数`N`使得`N % K == 0`，我们就先来看看对`K`取余的一些特点。对`K`取余的结果有K个，当结果为0的时候就是整除，所以除去整除的情况，就有K-1个结果。我们从小到大构建K个数，如果K个数当中有一个的余数是0，那么这个就是答案了；如果没有为0的，按照抽屉原理，那么肯定会有两个余数是相同的。因为余数直接是有关联的（后面证明），所以说明后面是会不断循环的，因此也就不存在整除的情况。

&emsp;&emsp;重新梳理下思路，优化的方法就是把穷搜变成了只计算K个数的余数，K个数能找到整除的就直接是答案，不能找到的话后面也不会找到了。下面就来看看更为严谨的数学证明，我们要证明余数直接是有关联的。假设$f(n) = n个1组成的数$，$g(n) = f(n) % K$，则有：
$$
\begin{align}f(n) =& f(n-1) \times 10 + 1 \\g(n) =& (f(n-1) \times 10 + 1) \% K \\=& (f(n-1) \times 10) \% K + 1 \% K \\=& f(n-1) \% K \times 10 \% K + 1 \%K \\=& g(n-1) \times 10 \% K + 1 \% K \\=& (g(n-1) \times 10 + 1) \% K \end{align}
$$
&emsp;&emsp;上面的$g(n)$实际上就是余数，所以通过这个证明可以很清晰地看到余数直接的关系。

&emsp;&emsp;除了上面缩小了搜索范围这个优化，还有一个trick也可以利用起来。我们知道以1结尾的数字肯定不会是偶数，所以2的倍数都不能被整除。同时我们也知道5的倍数一定是以0或5结尾，所以仅包含1的数字也不能整除。我们选取`K` 的最后一位，如果不在`1, 3, 7, 9`这几个数中，那么就肯定无法整除。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int smallestRepunitDivByK(int K) {
        set<int> excluded {1, 3, 7, 9};
        int lastDigit = K % 10;
        if (excluded.count(lastDigit) == 0) {
            return -1;
        }
        
        int temp = 0;
        for (int i = 1; i <= K; i++) {
            temp = (temp * 10 + 1) % K;
            if (temp == 0) {
                return i;
            }
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目重在分析，coding方面没有太多的技巧。在处理搜索类题目的时候，优化的方向是减少搜索的范围。而在处理余数的时候，要利用到余数是在一个范围的这个特点。这道题目的分享到这里，谢谢！
