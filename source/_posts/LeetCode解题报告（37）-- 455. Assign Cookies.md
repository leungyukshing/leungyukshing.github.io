---
title: LeetCode解题报告（37）-- 455. Assign Cookies
tags:
  - LeetCode
abbrlink: 51353
date: 2018-11-26 09:04:39
---
## Problem
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size `sj`. If `sj >= gi`, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Note:**
You may assume the greed factor is always positive.
You cannot assign more than one cookie to one child.
<!-- more -->

**Example 1:**
```
Input: [1,2,3], [1,1]

Output: 1

Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```

**Example 2:**
```
Input: [1,2], [1,2,3]

Output: 2

Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2.
You have 3 cookies and their sizes are big enough to gratify all of the children,
You need to output 2.
```

## Analysis
&emsp;&emsp;这道题是一道很常见的算法题——分配问题。问题要求我们将曲奇分配给孩子，每个曲奇有一个值，每个孩子也有一个满足值，只有当曲奇的值大于等于孩子满足值得时候，孩子才能满足，要求的是最大满足数量。这看上去是一个动态规划问题，但实际上要更简单。我的思路是，首先将两个数组进行排序（从小到大），然后同时开始遍历，思想就是**用最贴近满足值的饼干去满足孩子**，这样就不会有**浪费**的情况出现了。

## Solution
&emsp;&emsp;使用系统提供给的`sort`函数对两个数组进行排序，然后使用两个下标同时遍历两个数组，当曲奇满足孩子后，两者同时往后移，否则，只有曲奇数组往后移，这里要注意越界的问题。

## Code
```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int contentNumber = 0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int size1 = g.size();
        int size2 = s.size();

        int i = 0, j = 0;
        while (i < size1 && j < size2) {
            if (g[i] <= s[j]) {
                i++;
                j++;
                contentNumber++;
            }
            else {
                j++;
            }
        }
        return contentNumber;
    }
};
```
**运行时间：**约36ms，超过44.46%的CPP代码。

## Summary
&emsp;&emsp;这题看上去有点难，但实际上做完排序后，就是一个比较的问题了。解决问题的关键在于如何找到一种**最不浪费**的方式去分配饼干。即**最小满足原则**。本题的分析到此为止，谢谢您的支持！
