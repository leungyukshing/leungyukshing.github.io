---
title: LeetCode解题报告（371) -- 204. Count Primes
tags:
  - LeetCode
mathjax: true
date: 2021-05-12 19:44:13

---

## Problem

Count the number of prime numbers less than a non-negative number, `n`.

<!-- more -->

**Example 1:**

```
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

**Example 2:**

```
Input: n = 0
Output: 0
```

**Example 3:**

```
Input: n = 1
Output: 0
```



**Constraints:**

- `0 <= n <= 5 * 106`

------

## Analysis

&emsp;&emsp;这是一道非常经典的质数题目，给定了一个上限，问这个上限内有多少个质数。这道题目的解法非常多，首先是最暴力的解法，遍历这个上限内的所有数字，然后判断这个数是否质数。

&emsp;&emsp;判断质数的方法也很多：

1. 可以从1遍历到`n`，看看中间有没有能整除的因子；
2. 可以从1遍历到`n / 2`，看看中间有没有能整除的因子；
3. 可以从1遍历到$\sqrt{n}$，看看中间有没有能整除的因子

&emsp;&emsp;无论采用哪种判断质数的方法，复杂度并没有降低很多，上面的整体思路还是爆搜。下面有一种效率比较高的方法——标记法。

&emsp;&emsp;标记法的思想是如果我知道2是一个质数，那么2的所有倍数都不是质数；同样地，如果我知道3是质数，那么3的所有倍数都不是质数；接下来我需要考虑4吗？不需要，因为4是2的倍数，前面已经排除掉了；然后考虑5，5是质数，所以5的所有倍数都不是质数，这里标记的时候，就不需要标记10、15、20，因为10和20已经在前面处理2的时候标记过了，15已经在前面处理3的时候标记过了；同样地6也不用考虑，以此类推，我们很快就能够把所有非质数标出来。最后再遍历一遍看看那些是质数即可。

&emsp;&emsp;上面说的这个思路应该很好理解，难点在于处理某个数的倍数时，怎么知道从那个数开始。还是以上面的例子，处理5的时候，实际上是从25开始的，往后多看几个数字就会发现，当处理某个数`p`的倍数时，是从$p^2$开始处理，所以处理的数是$p^2$、$p^2 + p$、$p^2 + 2p$以此类推。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int countPrimes(int n) {
        if (n == 0) {
            return 0;
        }
        bool isPrime[n];
        for (int i = 2; i < n; i++) {
            isPrime[i] = true;
        }
        
        for (int i = 2; i * i < n; i++) {
            if (!isPrime[i]) {
                continue;
            }
            for (int j = i * i; j < n; j += i) {
                isPrime[j] = false;
            }
        }
        
        int result = 0;
        for (int i = 2; i < n; i++) {
            if (isPrime[i]) {
                result++;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道经典题目是很有价值的，后面提供的最优解法可能比较难直接想出来，在这里总结过了才知道怎么做。这道题目的分享到这里，感谢你的支持！
