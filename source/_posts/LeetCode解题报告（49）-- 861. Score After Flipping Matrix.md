---
title: LeetCode解题报告（49）-- 861. Score After Flipping Matrix
tags:
  - LeetCode
date: 2019-09-14 23:45:55

---

## Problem

We have a two dimensional matrix `A` where each value is `0`or `1`.

A move consists of choosing any row or column, and toggling each value in that row or column: changing all `0`s to `1`s, and all `1`s to `0`s.

After making any number of moves, every row of this matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.

Return the highest possible score.

<!-- more -->

**Example 1:**

```
Input: [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation:
Toggled to [[1,1,1,1],[1,0,0,1],[1,1,1,1]].
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39
```

**Note:**

1. `1 <= A.length <= 20`
2. `1 <= A[0].length <= 20`
3. `A[i][j]` is `0` or `1`.

------

## Analysis

&emsp;&emsp;这道题是关于矩阵的数学计算，题目要求我们按照一定规则翻转矩阵的行和列，从而使得每一行所组成的二进制数之和最大。对这种最优化问题，一般有两个方向：一是动态规划，二是贪心算法。动态规划在这道题大概率行不通，因为我们翻转的规则过于复杂，并没有一个很明确的方向。我们既可以按行翻转，也可以按列翻转，而且翻转的次数也是多次的，因此我就往贪心算法的方向去思考。

&emsp;&emsp;贪心的目的是让每一行组成的二进制数最大，这个比较容易实现，我们要求最高位都为1就能够实现（这个总是能够通过行翻转实现的）。然后这道题的难点就浮现了，我们很难去对一个数剩余的位找到一种贪心的做法，因为列翻转总是会影响其他数字。这个时候我们需要从整体的角度看待这个问题，前面说到我们想要每一行所组成的数最大，但实际上从按位相加的角度来看，行数是不重要的（因为最后计算的时候都是按照该位的值加起来），所以我们的贪心策略可以定义为：每一列中尽可能多的1。

&emsp;&emsp;至此，这道题的贪心策略出来了，分为两步：

1. 通过行翻转，让每一行的最高位（第1个数为1）
2. 让每一列的1多于0

------

## Solution

&emsp;&emsp;第一个贪心策略很容易实现，第二个策略的实现方式是先统计1的数量，判断是否多于半数。在下边的实现中，为了讲解的需要我将使用贪心算法操作的步骤和最后计算的步骤分开来写，实际上，我们可以在进行列翻转的同时进行计算，这样能够减少代码量。

------

## Code

```c++
class Solution {
public:
    int matrixScore(vector<vector<int>>& A) {
        int m = A.size();
        int n = A[0].size();
        
        // operate
        
        // flip row
        for (int i = 0; i < m; i++) {
            if (A[i][0] == 0) {
                for (int j = 0; j < n; j++) {
                    A[i][j] = !A[i][j];
                }
            }
        }
        
        // flip column
        for (int j = 0; j < n; j++) {
            int count = 0;
            for (int i = 0; i < m; i++) {
                if (A[i][j] == 1) {
                    count++;
                }
            }
            // more 1 than 0
            if (count > m / 2) {
                continue;
            }
            // more 0 than 1
            else {
                for (int i = 0; i < m; i++) {
                    A[i][j] = !A[i][j];
                }
            }
        }
        
        // compute
        int result = 0;
        for (int i = 0; i < m; i++) {
            int sum = 0;
            for (int j = 0; j < n; j++) {
                sum += A[i][j] * pow(2, n - j - 1);
            }
            result += sum;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道有关贪心算法的题目，有趣之处在于它用到了两个贪心算法。其中第一个是比较常见的，通过极大化最高位来使一个数最大，但是第二个就需要对题目的整体把握。希望这个分享能够帮助到你，欢迎大家转发、评论，谢谢您的支持！
