---
title: LeetCode解题报告（197）-- 948. Bag of Tokens
tags:
  - LeetCode
mathjax: true
abbrlink: 43246
date: 2020-10-25 18:36:02
---

## Problem

You have an initial **power** of `P`, an initial **score** of `0`, and a bag of `tokens` where `tokens[i]` is the value of the `ith` token (0-indexed).

Your goal is to maximize your total **score** by potentially playing each token in one of two ways:

- If your current **power** is at least `tokens[i]`, you may play the `ith` token face up, losing `tokens[i]` **power** and gaining `1` **score**.
- If your current **score** is at least `1`, you may play the `ith` token face down, gaining `tokens[i]` **power** and losing `1` **score**.

Each token may be played **at most** once and **in any order**. You do **not** have to play all the tokens.

Return *the largest possible **score** you can achieve after playing any number of tokens*.

<!-- more -->

**Example 1:**

```
Input: tokens = [100], P = 50
Output: 0
Explanation: Playing the only token in the bag is impossible because you either have too little power or too little score.
```

**Example 2:**

```
Input: tokens = [100,200], P = 150
Output: 1
Explanation: Play the 0th token (100) face up, your power becomes 50 and score becomes 1.
There is no need to play the 1st token since you cannot play it face up to add to your score.
```

**Example 3:**

```
Input: tokens = [100,200,300,400], P = 200
Output: 2
Explanation: Play the tokens in this order to get a score of 2:
1. Play the 0th token (100) face up, your power becomes 100 and score becomes 1.
2. Play the 3rd token (400) face down, your power becomes 500 and score becomes 0.
3. Play the 1st token (200) face up, your power becomes 300 and score becomes 1.
4. Play the 2nd token (300) face up, your power becomes 0 and score becomes 2.
```

**Constraints:**

- `0 <= tokens.length <= 1000`
- `0 <= tokens[i], P < 104`

------

## Analysis

&emsp;&emsp;题目的意思比较简单，有两个东西**score**和**power**，这两个东西是可以互换的，我们一开始有一定数量的power，要求通过翻牌操作后，能获得的最大的score。首先我们可以用power去换score，在给定的tokens数组中，消耗tokens就能够获得一个score。同时我们有score的话，也可以根据tokens去换取power。

&emsp;&emsp;所以这里面就有一个逻辑就是，可以用较少的power，先去换取一个score，然后用这个score去换回较多的power。这里面就有一个贪心的策略了，当我们有power的时候，用最少的tokens去换score，当power不够了，就用score去换最多的power，直到score再也换取不到power了。

------

## Solution

&emsp;&emsp;首先排序，然后从小的tokens开始换，换取尽可能多的score，不够的时候从后面开始换power，遍历的过程维护全局最优解即可。

------

## Code

```c++
class Solution {
public:
    int bagOfTokensScore(vector<int>& tokens, int P) {
        int left = 0, right = tokens.size() - 1;
        sort(tokens.begin(), tokens.end());
        
        int result = 0, current = 0;
        while (left <= right) {
            while (left <= right && tokens[left] <= P) {
                P -= tokens[left++];
                current++;
                result = max(result, current);
            }
            if (left > right || current == 0) {
                break;
            }
            current--;
            P += tokens[right--];
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;最近很久没有碰到过贪心算法的题目了，这道就是很明显的贪心，分别从两个方向对数组进行操作，其余的部分并没有很大难度。这道题目的分享到这里，谢谢！
