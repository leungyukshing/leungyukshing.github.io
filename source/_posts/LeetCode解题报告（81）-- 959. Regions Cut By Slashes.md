---
title: LeetCode解题报告（81）-- 959. Regions Cut By Slashes
tags:
  - LeetCode
date: 2019-12-24 23:56:41
mathjax: true

---

## Problem

In a N x N `grid` composed of 1 x 1 squares, each 1 x 1 square consists of a `/`, `\`, or blank space.  These characters divide the square into contiguous regions.

(Note that backslash characters are escaped, so a `\` is represented as `"\\"`.)

Return the number of regions.

<!-- more -->

**Example 1:**

```
Input:
[
  " /",
  "/ "
]
Output: 2
Explanation: The 2x2 grid is as follows:
```

![ex1](https://assets.leetcode.com/uploads/2018/12/15/1.png)

**Example 2:**

```
Input:
[
  " /",
  "  "
]
Output: 1
Explanation: The 2x2 grid is as follows:
```

![ex2](https://assets.leetcode.com/uploads/2018/12/15/2.png)

**Example 3:**

```
Input:
[
  "\\/",
  "/\\"
]
Output: 4
Explanation: (Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.)
The 2x2 grid is as follows:
```

![ex3](https://assets.leetcode.com/uploads/2018/12/15/3.png)

**Example 4:**

```
Input:
[
  "/\\",
  "\\/"
]
Output: 5
Explanation: (Recall that because \ characters are escaped, "/\\" refers to /\, and "\\/" refers to \/.)
The 2x2 grid is as follows:
```

![ex4](https://assets.leetcode.com/uploads/2018/12/15/4.png)

**Example 5:**

```
Input:
[
  "//",
  "/ "
]
Output: 3
Explanation: The 2x2 grid is as follows:
```

![ex5](https://assets.leetcode.com/uploads/2018/12/15/5.png)

**Note:**

1. `1 <= grid.length == grid[0].length <= 30`
2. `grid[i][j]` is either `'/'`, `'\'`, or `' '`.

------

## Analysis

&emsp;&emsp;这道题目看上去比较困难，涉及到图形区域的连通性，之前没碰到过类似的题目。首先题目讲的是连通性问题，我们可以优先采取并查集来解题。这点在之前有关连通性的题目中已经讲过。这道题的难点在于如何把题目的输入转化为区域的连通性信息。

&emsp;&emsp;对于每一个小方格，题目的切分方法是对角线切分，也就是说，最小的区域是四分之一的正方形--一个小三角形。所以我们计算连通性的时候单位需要用小三角形，而不是一整个方格。

![Mininum Unit](/images/Regions_Cut_By_Slashes.png)

&emsp;&emsp;在$N \times N$的方格中，总共有$4 \times N \times N$个小的三角形。我们对每个方格中的四个三角形进行一定的编号（这个编号是自定义的，但是这里定义好后，后面的计算也要统一）。有了这个编号后，我们可以首先通过`(i,j)`来定位到某个方格，然后再通过加上偏置访问到对应的三角形。

&emsp;&emsp;接下来的问题就是建立连通性。连通性的建立有两个：

+ 一是方格与方格之间的三角形建立连通性；
+ 二是方格内部的三角形之间建立连通性。

&emsp;&emsp;方格与方格之间的连通性是**天然的**，因为对角线的切分并不会影响两个方格之间的两个三角形的连通性，所以我们可以直接建立连通性。

&emsp;&emsp;方格内的连通性的建立就需要依赖于题目给出的切割线的位置。切割线的位置有两种：

1. `/`切割，则`n`和`n+3`连通，`n+1`和`n+2`连通；
2. `\\`切割，则`n`和`n+1`连通，`n+3`和`n+2`连通。

&emsp;&emsp;至此，整个建立连通性的过程已经分析完毕，最后我们只需要寻找有几个根节点就知道有几个连通区域了。

------

## Solution

&emsp;&emsp;这里的并查集应用同样是使用一个一维数组即可。长度是小三角形的数量。对于`(i,j)`位置的第$k(k < 4)$个三角形，下标计算为：$4 * (i * N + j)$。

&emsp;&emsp;最后判断根节点的条件是$parent[i] == i$。

------

## Code

```c++
class Solution {
public:
    int regionsBySlashes(vector<string>& grid) {
        N = grid.size();
        M = 4 * N * N;
        init();
        
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                int n =  4 * (i * N + j);
                if (i < N - 1) {
                    int n_down = 4 * ((i + 1) * N + j);
                    merge(n + 2, n_down);
                }
                if (j < N - 1) {
                    int n_right = 4 * (i * N + j + 1);
                    merge(n + 1, n_right + 3);
                }
            }
        }
        
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                int n =  4 * (i * N + j);
                if (grid[i][j] != '/') {
                    merge(n, n + 1);
                    merge(n + 2, n + 3);
                }
                if (grid[i][j] != '\\') {
                    merge(n, n + 3);
                    merge(n + 1, n + 2);
                }
            }
        }
        
        int result = 0;
        for (int i = 0; i < M; i++) {
            //cout << "i: " << i << ", val: " << parent[i] << endl;
            if (find(i) == i) {
                result++;
            }
        }
        return result;
    }
private:
    int parent[3600];
    int N, M; // N is grid size, M is one-dimensional size
    void init() {
        for (int i = 0; i < M; i++) {
            parent[i] = i;
        }
    }
    
    int find(int i) {
        return i == parent[i] ? i : parent[i] = find(parent[i]);
    }
    
    void merge(int x, int y) {
        //cout << "merge " << x << " and " << y << endl;
        int parent_x = find(x);
        int parent_y = find(y);
        parent[parent_x] = parent_y;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题目是并查集的具体应用，题目的原理不难，但是输入的处理与转化需要技巧。难点在于分析出以小三角形为连通单位进行计算，同时在计算中为了方便可以将多维的数据转化为一维处理。这道题目就和大家分享到这里，欢迎大家评论、转发，谢谢您的支持！
