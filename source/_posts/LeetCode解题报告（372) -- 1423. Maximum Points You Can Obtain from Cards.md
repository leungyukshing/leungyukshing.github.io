---
title: LeetCode解题报告（372) -- 1423. Maximum Points You Can Obtain from Cards
tags:
  - LeetCode
mathjax: true
date: 2021-05-12 20:11:39

---

## Problem

There are several cards **arranged in a row**, and each card has an associated number of points The points are given in the integer array `cardPoints`.

In one step, you can take one card from the beginning or from the end of the row. You have to take exactly `k` cards.

Your score is the sum of the points of the cards you have taken.

Given the integer array `cardPoints` and the integer `k`, return the *maximum score* you can obtain.

<!-- more -->

**Example 1:**

```
Input: cardPoints = [1,2,3,4,5,6,1], k = 3
Output: 12
Explanation: After the first step, your score will always be 1. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.
```

**Example 2:**

```
Input: cardPoints = [2,2,2], k = 2
Output: 4
Explanation: Regardless of which two cards you take, your score will always be 4.
```

**Example 3:**

```
Input: cardPoints = [9,7,7,9,7,7,9], k = 7
Output: 55
Explanation: You have to take all the cards. Your score is the sum of points of all cards.
```

**Example 4:**

```
Input: cardPoints = [1,1000,1], k = 1
Output: 1
Explanation: You cannot take the card in the middle. Your best score is 1. 
```

**Example 5:**

```
Input: cardPoints = [1,79,80,1,1,1,200,1], k = 3
Output: 202
```



**Constraints:**

- `1 <= cardPoints.length <= 10^5`
- `1 <= cardPoints[i] <= 10^4`
- `1 <= k <= cardPoints.length`

------

## Analysis

&emsp;&emsp;题目给出一个数组，还有一个数字`k`代表操作的次数。每次操作只能从最左边或最右边取出一个数，问取出数之和最大是多少。这种题目贪心肯定是行不通的，dp又不太好做，因为取数是两边取，我们不妨转化一下思路，题目要求的是从两边取走`k`个数字，实际上也就是在数组中间保留长度为`n - k`连续的一段数字，要求这段数字最小。

&emsp;&emsp;这样思考题目一下子变简单了，维护一个滑动窗口，要求里面的值最小。假设数组总和是`sum`，窗口的和是`windowSum`，滑动过程中维护`result = max(result, sum - windowSum)`。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int n = cardPoints.size();
        int size = n - k;
        int left = 0, right = 0;
        int windowSum = 0;
        
        int total = accumulate(cardPoints.begin(), cardPoints.end(), 0);
        while (size--) {
            windowSum += cardPoints[right];
            right++;
        }
        
        int result = total - windowSum;
        for (int i = 0; i < k; i++) {
            windowSum -= cardPoints[left];
            left++;
            windowSum += cardPoints[right];
            right++;
            // cout << windowSum << endl;
            result = max(result, total - windowSum);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点在于转换思路，从反面去思考，转换后就是一个非常简单的滑动窗口问题。这道题目的分享到这里，感谢你的支持！
