---
title: LeetCode解题报告（481）-- 1182. Shortest Distance to Target Color
mathjax: true
date: 2022-01-01 12:41:15
---

## Problem

You are given an array `colors`, in which there are three colors: `1`, `2` and `3`.

You are also given some queries. Each query consists of two integers `i` and `c`, return the shortest distance between the given index `i` and the target color `c`. If there is no solution return `-1`.

<!-- more -->

**Example 1:**

```
Input: colors = [1,1,2,1,3,2,2,3,3], queries = [[1,3],[2,2],[6,1]]
Output: [3,0,3]
Explanation: 
The nearest 3 from index 1 is at index 4 (3 steps away).
The nearest 2 from index 2 is at index 2 itself (0 steps away).
The nearest 1 from index 6 is at index 3 (3 steps away).
```

**Example 2:**

```
Input: colors = [1,2], queries = [[0,3]]
Output: [-1]
Explanation: There is no 3 in the array.
```



**Constraints:**

- `1 <= colors.length <= 5*10^4`
- `1 <= colors[i] <= 3`
- `1 <= queries.length <= 5*10^4`
- `queries[i].length == 2`
- `0 <= queries[i][0] < colors.length`
- `1 <= queries[i][1] <= 3`

---

## Analysis

&emsp;&emsp;题目给出一个`colors`数组代表每个位置的颜色，然后还有一个`queries`数组，里面每个query包括`[i, c]`，代表查找距离`i`位置最近的颜色`c`的距离，要求返回查询的答案。实际上这道题目可以转化为，求每个位置上距离三种颜色最近的距离各是多少，如果我们计算出来这个的话，每个query都可以很快计算出来。

&emsp;&emsp;距离某个位置最近，只有两种可能，一个在左边，一个在右边，所以需要两次处理，我们先来计算color在位置`i`的右边的情况，左边是同理的。我们用一个大小为3的`rightMost`数组表示每个颜色当前出现过最靠右边的坐标（coding时再加上1，是为了更新方便），这个数组的意义是这个下标左边的位置就不需要care了，因为所有小于这个下标的位置计算右边的距离时，最短距离不可能超过这个数组。比如exmaple1中，遍历到`i = 4`之后，`rightMost = [3, 2, 4]`，但是上面说到coding时加1，所以应该是`rightMost=[4, 3, 5]`，也就是说当下标小于4时，如果计算下标右边与颜色1的距离，最短的距离肯定是和`4 - 1 = 3`去计算，当下标小于3时，如果计算下标右边与颜色2的距离，最短的距离肯定是和`3 - 1 = 2`去计算，当下标小于5时，如果计算下标右边与颜色3的距离，最短的距离肯定是和`5 - 1 = 4`去计算。那么当处理`i = 5`时，color是2，所以我们要把从位置`[3, 5]`与color 2的距离都更新一遍。

&emsp;&emsp;同理，处理color在位置`i`左边的情况，我们可以用一个大小为3的`leftMost`数组，这里在更新时就需要注意取一个极小值，实际的意义就是对于某个位置`i`，看看是在它的左边找到color比较近，还是在它的右边找到color比较近。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> shortestDistanceColor(vector<int>& colors, vector<vector<int>>& queries) {
        int size = colors.size();
        vector<int> rightMost(3, 0);
        vector<int> leftMost(3, size - 1);
        vector<vector<int>> distance(3, vector<int>(size, -1));
    
        for (int i = 0; i < size; ++i) {
            int color = colors[i] - 1;
            for (int j = rightMost[color]; j < i + 1; j++) {
                distance[color][j] = i - j;
            }
            rightMost[color] = i + 1;
        }
        
        for (int i = size - 1; i >= 0; --i) {
            int color = colors[i] - 1;
            for (int j = leftMost[color]; j > i - 1 ; --j) {
                if (distance[color][j] == -1 || distance[color][j] > j - i) {
                    distance[color][j] = j - i;
                }
            }
            leftMost[color] = i - 1;
        }
        
        vector<int> result;
        for (vector<int> &query: queries) {
            result.push_back(distance[query[1] - 1][query[0]]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目有点像dp但又不是很典型的dp，可以直接理解为用两个数组先存储左右两边的信息，再计算。这道题目的分享到这里，感谢你的支持！

