---
title: 小红与粉刷匠
mathjax: true
date: 2022-04-11 23:43:18

---

## Problem

&emsp;&emsp;小红遇到了一名粉刷匠。这名粉刷匠有三种颜料，分别是红、黄、蓝。为了方便， 这三种颜料分别命名为 A，B，C。

&emsp;&emsp;现在，粉刷匠正在粉刷一面长度为3n 的墙壁。粉刷完之后，三种颜料的数目都相同。由于小红一不小心踢到了颜料桶，使得这面墙的每个地方都被染上了三种颜料中的其中一种，这很让粉刷匠头疼。

&emsp;&emsp;粉刷匠每次可以选择一段连续的墙壁进行粉刷，即全部粉刷上同一种颜色（A,B,C 三种中的其中一种）。粉刷匠想知道，他最少需要多少次粉刷才能使得三种颜料的数目都相同？

<!-- more -->

**Example 1:**

```
Input
ABACBC

Output
0

Explanation
A、B、C的数目都是2，不需要粉刷
```

**Example 2:**

```
Input
AAABBBBAC

Output
1

Explanation
把区间[6, 8]全部刷成C
```

**Example 3:**

```
Input
CAACBCBCC

Output
2

Explanation
先把区间[3, 3]粉刷成B，得到CAABBCBCC。然后把区间[8, 8]粉刷成为A
```

---

## Analysis

&emsp;&emsp;这道题目一开始很难入手，因为颜色不一定是连续的，所以选择哪块区间和刷哪种颜色都不知道。但是有一个很重要的信息就是，我们知道最后每种颜色有多少块。无非有三种情况：

1. 当三种颜色的数量都一样，那么直接就是答案了，返回0；
2. 当一种颜色多了，其余两种颜色少了，那必定要刷超过2次，因为数量少的每种颜色至少各刷一次去增加数量；
3. 当两种颜色多了，一种颜色少了，至少刷一次去增加数量少的颜色。

&emsp;&emsp;然后我们根据题目的例子发现，刚好对应了上面的三种情况，答案不超过2，这似乎是一个规律。于是我们可以思考下刷的次数到底有没有可能超过2呢？假设我刷超过2次，那么减少的颜色的种类就超过2种，因为每一次刷至少减少一种颜色。那么减少的颜色超过2种说明三种颜色的数量都减少了，这个是不符合题意的，所以可以得到刷的次数最多就是2。

&emsp;&emsp;有了这个规律就很好做了，我们只需要统计出每种颜色的数字，然后把多余的颜色找出来。再用滑动窗口去找是否满足有多余颜色的子串，如果有，说明刷一次就能解决，如果没有，说明要刷2次。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
#include <iostream>
#include <string>
using namespace std;

int minimumOperation(string str) {
    int n = str.size();
    int count[3] = {0};

    for (char ch: str) {
        ++count[ch - 'A'];
    }

    int need[3] = {0};
    int m[3] = {0};
    int needValid = 0;
    for (int i = 0; i < 3; ++i) {
        if (count[i] > n / 3) {
            need[i] = count[i] - n / 3;
            ++needValid;
        }
    }

    if (needValid == 0) {
        return 0;
    }
    if (needValid == 1) {
        return 2;
    }

    int left = 0, right = 0;
    int valid = 0;
    while (right < n) {
        char ch = str[right++];
        if (need[ch - 'A'] > 0) {
            ++m[ch - 'A'];
            if (m[ch - 'A'] == need[ch - 'A']) {
                ++valid;
            }
        }
        if (valid == needValid) {
            // cout << left << " " << right << endl;
            return 1;
        }

        while (m[ch - 'A'] > need[ch - 'A']) {
            ch = str[left++];
            if (m[ch - 'A'] == need[ch - 'A']) {
                --valid;
            }
            --m[ch - 'A'];
        }
    }
    return 2;
}

int main() {
    string str = "";
    cin >> str;
    cout << minimumOperation(str) << endl;
    return 0;
}
```

------

## Summary

&emsp;&emsp;这道题目难度不小，首先需要通过找规律的方法来确定最大的粉刷次数是2，然后分析出不同情况的刷法，最后利用滑动窗口来寻找是否存在一次粉刷的情况，可谓是结合了分析和coding skill，是一道难得的好题！这道题目的分享到这里，感谢你的支持！
