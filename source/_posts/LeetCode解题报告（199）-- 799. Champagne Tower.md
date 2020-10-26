---
title: LeetCode解题报告（199）-- 799. Champagne Tower
tags:
  - LeetCode
mathjax: true
date: 2020-10-27 00:41:30

---

## Problem

We stack glasses in a pyramid, where the **first** row has `1` glass, the **second** row has `2` glasses, and so on until the 100th row.  Each glass holds one cup (`250`ml) of champagne.

Then, some champagne is poured in the first glass at the top.  When the topmost glass is full, any excess liquid poured will fall equally to the glass immediately to the left and right of it.  When those glasses become full, any excess champagne will fall equally to the left and right of those glasses, and so on.  (A glass at the bottom row has its excess champagne fall on the floor.)

<!-- more -->

For example, after one cup of champagne is poured, the top most glass is full.  After two cups of champagne are poured, the two glasses on the second row are half full.  After three cups of champagne are poured, those two cups become full - there are 3 full glasses total now.  After four cups of champagne are poured, the third row has the middle glass half full, and the two outside glasses are a quarter full, as pictured below.

![exmaple](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/09/tower.png)

Now after pouring some non-negative integer cups of champagne, return how full the `jth` glass in the `ith` row is (both `i` and `j` are 0-indexed.)

**Example 1:**

```
Input: poured = 1, query_row = 1, query_glass = 1
Output: 0.00000
Explanation: We poured 1 cup of champange to the top glass of the tower (which is indexed as (0, 0)). There will be no excess liquid so all the glasses under the top glass will remain empty.
```

**Example 2:**

```
Input: poured = 2, query_row = 1, query_glass = 1
Output: 0.50000
Explanation: We poured 2 cups of champange to the top glass of the tower (which is indexed as (0, 0)). There is one cup of excess liquid. The glass indexed as (1, 0) and the glass indexed as (1, 1) will share the excess liquid equally, and each will get half cup of champange.
```

**Example 3:**

```
Input: poured = 100000009, query_row = 33, query_glass = 17
Output: 1.00000
```

**Constraints:**

- `0 <= poured <= 109`
- `0 <= query_glass <= query_row < 100`

------

## Analysis

&emsp;&emsp;这道题目是以倒香槟为背景的，看到这个图我第一时间想到了杨辉三角。虽然这道题目和杨辉三角没什么关系，但是计算的过程还是有点像的。下一层会依赖上一层的结果。讲到这里，是不是又有点dp的感觉了？我们不深究他是不是典型的dp，反正从上往下，从左到右计算就是了。

&emsp;&emsp;总的酒量是确定的，在计算过程中，我们假设这个酒是可以暂时放到杯子里面的（多出的在下一层的循环分配过去）。举个例子，如果总的酒量是10，那么在遍历第一层的时候，第一层的这个杯子的值就是10，等到遍历第二层的时候，再用10减去酒杯的容量1，再做均分。

&emsp;&emsp;根据分配的规律可以知道，每个杯子都是由两个上面一层的酒杯多余的酒分配下来的（除去第一层、每层的头尾外）。所以我们只需要判断上一层的酒杯是否有多余的酒，如果有就均分2份，然后一份加到当前的这个酒杯中。

------

## Solution

&emsp;&emsp;因为题目给定了行数和列数的范围，所以可以直接申请10*10的二维空间。

&emsp;&emsp;需要注意，因为我们在运算的时候是假设酒都是可以暂时放到酒杯中的，如果当前的这个酒杯满了，可能他还有多余的酒，所以在返回答案的时候需要取`min(1, f[row][col])`。

------

## Code

```c++
class Solution {
public:
    double champagneTower(int poured, int query_row, int query_glass) {
        vector<vector<double>> f(100, vector<double>(100, 0));
        f[0][0] = poured;
        for (int i = 1; i <= query_row; i++) {
            for (int j = 0; j <= i; j++) {
                if (j > 0 && f[i - 1][j - 1] > 1) {
                    f[i][j] += (f[i - 1][j - 1] - 1) / 2.0;
                }
                if (f[i - 1][j] > 1) {
                    f[i][j] += (f[i - 1][j] - 1) / 2.0;
                }
            }
        }
        return min(1.0, f[query_row][query_glass]);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不算大，按照题目的计算思路编程即可，但是要注意头尾的边界情况。这道题目的分享到这里，谢谢！
