---
title: LeetCode解题报告（422）-- 1861. Rotating the Box
mathjax: true
date: 2021-12-13 14:13:24
---

## Problem

You are given an `m x n` matrix of characters `box` representing a side-view of a box. Each cell of the box is one of the following:

- A stone `'#'`
- A stationary obstacle `'*'`
- Empty `'.'`

<!-- more -->

The box is rotated **90 degrees clockwise**, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity **does not** affect the obstacles' positions, and the inertia from the box's rotation **does not** affect the stones' horizontal positions.

It is **guaranteed** that each stone in `box` rests on an obstacle, another stone, or the bottom of the box.

Return *an* `n x m` *matrix representing the box after the rotation described above*.

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcodewithstones.png)

```
Input: box = [["#",".","#"]]
Output: [["."],
         ["#"],
         ["#"]]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode2withstones.png)

```
Input: box = [["#",".","*","."],
              ["#","#","*","."]]
Output: [["#","."],
         ["#","#"],
         ["*","*"],
         [".","."]]
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2021/04/08/rotatingtheboxleetcode3withstone.png)

```
Input: box = [["#","#","*",".","*","."],
              ["#","#","#","*",".","."],
              ["#","#","#",".","#","."]]
Output: [[".","#","#"],
         [".","#","#"],
         ["#","#","*"],
         ["#","*","."],
         ["#",".","*"],
         ["#",".","."]]
```

**Constraints:**

- `m == box.length`
- `n == box[i].length`
- `1 <= m, n <= 500`
- `box[i][j]` is either `'#'`, `'*'`, or `'.'`.

------

## Analysis

&emsp;&emsp;这道题目给出一个矩阵，里面包含三种符号：

+ `#`代表石头；
+ `*`代表一个静止的障碍；
+ `.`代表空。

&emsp;&emsp;题目要求我们把这个矩阵顺时针旋转90度，旋转后，静止的障碍保持不变，石头会往下掉，问反转完之后的结果是怎么样？

&emsp;&emsp;题目包括两个主要操作，一个是反转，另一个是石头往下掉。反转很容易，已经有tips，公式就是`result[j][m - i - 1] = box[i][j]`。重点在于控制石头往下掉。这道题目和[Candy Crush]()非常类似，其本质都是东西往下掉，但这道题目中难点在于有一个静止的障碍。

&emsp;&emsp;首先我们理清楚处理向下掉落这类问题的思路。一般来说我会采用双指针的形式，指针`j`用来遍历元素，而且必须是**从下到上**遍历，因为这样的顺序才符合掉落的情况。再用另一个指针`front_idx`来表示下一个可以填空的位置。如何理解这两个指针呢？

+ 如果当前这一列只有第一个元素是石头，其余的元素都是空，那么`front_idx`初始化为`n - 1`，当`j`遍历到`j = 0`的时候，直接把是否放到`front_idx`的位置上即可，`j`位置上的元素清空；
+ 如果当前这一列全是石头，那么`front_idx`和`j`在每一个循环都是一样的。

&emsp;&emsp;这道题比较难搞的是有一个静止的障碍，其实我们可以把这个静止的障碍看作是一个新的底部，相当于把`front_idx`再初始化一次。所以当`j`位置上是一个静止的障碍，那么`front_idx = j - 1`。

&emsp;&emsp;以上就是整个掉落的思路。

## Solution

&emsp;&emsp;&emsp;我们可以先处理掉落，再处理反转。对于原始矩阵来说，元素是向右边掉落的，处理结束后再根据反转的公式组成新的矩阵返回。

------

## Code

```c++
class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int m = box.size(), n = box[0].size();
        
        for (int i = 0; i < m; ++i) {
            int front_idx = n - 1;
            for (int j = n - 1; j >= 0; --j) {
                if (box[i][j] == '*') {
                    // obstacle, update front idx
                    front_idx = j - 1;
                } else if (box[i][j] == '#') {
                    if (front_idx > j) {
                        box[i][front_idx] = '#';
                        box[i][j] = '.';
                        front_idx--;
                    } else {
                        front_idx = j - 1;
                    }
                }
            }
        }
        
        vector<vector<char>> result(n, vector<char>(m));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                result[j][m - i - 1] = box[i][j];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目看上去有点复杂，但矩阵元素掉落的思路还是以双指针为主。针对静止障碍这种特殊的元素，新加判断逻辑即可。这道题目的分享到这里，感谢你的支持！
