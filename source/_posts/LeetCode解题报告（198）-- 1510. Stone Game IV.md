---
title: LeetCode解题报告（198）-- 1510. Stone Game IV
tags:
  - LeetCode
mathjax: true
abbrlink: 17143
date: 2020-10-26 18:54:26
---

## Problem

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are `n` stones in a pile.  On each player's turn, that player makes a *move* consisting of removing **any** non-zero **square number** of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer `n`. Return `True` if and only if Alice wins the game otherwise return `False`, assuming both players play optimally.

<!-- more -->

**Example 1:**

```
Input: n = 1
Output: true
Explanation: Alice can remove 1 stone winning the game because Bob doesn't have any moves.
```

**Example 2:**

```
Input: n = 2
Output: false
Explanation: Alice can only remove 1 stone, after that Bob removes the last one winning the game (2 -> 1 -> 0).
```

**Example 3:**

```
Input: n = 4
Output: true
Explanation: n is already a perfect square, Alice can win with one move, removing 4 stones (4 -> 0).
```

**Example 4:**

```
Input: n = 7
Output: false
Explanation: Alice can't win the game if Bob plays optimally.
If Alice starts removing 4 stones, Bob will remove 1 stone then Alice should remove only 1 stone and finally Bob removes the last one (7 -> 3 -> 2 -> 1 -> 0). 
If Alice starts removing 1 stone, Bob will remove 4 stones then Alice only can remove 1 stone and finally Bob removes the last one (7 -> 6 -> 2 -> 1 -> 0).
```

**Example 5:**

```
Input: n = 17
Output: false
Explanation: Alice can't win the game if Bob plays optimally.
```

**Constraints:**

- `1 <= n <= 10^5`

------

## Analysis

&emsp;&emsp;移走石头的博弈游戏，这个是系列题。大致的规则和前面一样，增加了一条规则：**移走的数量必须是平方数**。只要对方没有办法按照规则移走石头，则算胜出。这个问题实际上可以转化为如果现在有`n`个石头，当前玩家能不能胜出。

&emsp;&emsp;看到这里，开始有点dp的感觉了，不断地把父问题切分为子问题，而且肯定是先处理前面的，再处理后面的。因为在`n`确定的情况下，最优的策略都是一样的。所以我们只需要从0开始，一直往后遍历，每次检查在减去平方数之后，对方能不能获胜，如果对方有一次不能获胜，则说明当前玩家可以获胜了。

&emsp;&emsp;这里注意的是，不是判断对方获胜了，当前玩家就输，因为我们假设的是玩家会做出最优选择，比如有100种可能的情况下，对方赢99次，但是只要输1次，那么当前的玩家就是获胜的。

------

## Solution

&emsp;&emsp;简单的从左往右DP，没有复杂的coding。

------

## Code

```c++
class Solution {
public:
    bool winnerSquareGame(int n) {
        vector<bool> f(n + 1, 0);
        f[0] = false;
        for (int i = 0; i <= n; i++) {
            f[i] = false;
            for (int j = 1; j * j <= i; j++) {
                if (!f[i - j * j]) {
                    f[i] = true;
                    break;
                }
            }
        }
        return f[n];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是布尔类型的dp，之前好像比较少通过dp解决博弈问题的，这里也算是做一个总结。实际上dp的本质还是不变的，通过切分出子问题，用子问题的解去解决父问题。这道题目的分享到这里，谢谢！
