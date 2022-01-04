---
title: LeetCode解题报告（494）-- 1276. Number of Burgers with No Waste of Ingredients
mathjax: true
date: 2022-01-03 22:52:25
---

## Problem

Given two integers `tomatoSlices` and `cheeseSlices`. The ingredients of different burgers are as follows:

- **Jumbo Burger:** `4` tomato slices and `1` cheese slice.
- **Small Burger:** `2` Tomato slices and `1` cheese slice.

Return `[total_jumbo, total_small]` so that the number of remaining `tomatoSlices` equal to `0` and the number of remaining `cheeseSlices` equal to `0`. If it is not possible to make the remaining `tomatoSlices` and `cheeseSlices` equal to `0` return `[]`.

<!-- more -->

**Example 1:**

```
Input: tomatoSlices = 16, cheeseSlices = 7
Output: [1,6]
Explantion: To make one jumbo burger and 6 small burgers we need 4*1 + 2*6 = 16 tomato and 1 + 6 = 7 cheese.
There will be no remaining ingredients.
```

**Example 2:**

```
Input: tomatoSlices = 17, cheeseSlices = 4
Output: []
Explantion: There will be no way to use all ingredients to make small and jumbo burgers.
```

**Example 3:**

```
Input: tomatoSlices = 4, cheeseSlices = 17
Output: []
Explantion: Making 1 jumbo burger there will be 16 cheese remaining and making 2 small burgers there will be 15 cheese remaining.
```



**Constraints:**

- `0 <= tomatoSlices, cheeseSlices <= 107`

---

## Analysis

&emsp;&emsp;这道题目的背景是做汉堡，有两种汉堡，`Jumbo Burger`要4片tomato和1片cheese，`Small Burger`要2片tomato和1片cheese，题目给出了`tomatoSlices`和`cheeseSlices`的数量，问两种汉堡最多能做多少个，其中要求两种材料都没有剩余，如果做不到则返回空。

&emsp;&emsp;这个问题一下子好像很困难，但是两个变量，去组成两种汉堡，这个听起来很小小学的方程题目啊，再仔细一看这不就是鸡兔同笼问题吗。先列方程，假设能做`x`个Jumbo Burger，能做`y`个Small Burger，那么有`4x + 2y = tomatoSlices`，`x + y = cheeseSlices`。然后我们来看怎么判断能否做到。

&emsp;&emsp;先消元`y`，得到`2x = tomatoSlices - 2cheeseSlices`，所以这里要保证`tomatoSlices >= 2 * chessSlices`，因为要求解`x`，所以`tomatoSlices % 2 == 0`才能保证`x`有整数解。求解完`x`后代入计算`y`，`2y = 4cheeseSlices - tomatoSlices`，同理要保证`4 * cheesseSlices >= tomatoSlices`。因为要求解`y`，所以`tomatoSlices % 2 == 0`才能保证`y`有整数解。

&emsp;&emsp;把上面的条件都判断一遍，如果都满足要求就直接代入方程计算出`x`和`y`，否则就返回空。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> numOfBurgers(int t, int c) {
        if (t % 2 == 0 && c * 2 <= t && t <= c * 4)
            return {t / 2 - c, c * 2 - t / 2};
        return {};
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上就是一道数学题，在解方程的过程中需要注意合法的情况，保证大于0以及保证整数解。这道题目的分享到这里，感谢你的支持！

