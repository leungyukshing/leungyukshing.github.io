---
title: LeetCode解题报告（34）-- 118. Pascal's Triangle
tags:
  - LeetCode
abbrlink: 16155
date: 2018-11-19 15:57:16
---
## Problem
Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![Animation](/images/PascalTriangleAnimated2.gif)
In Pascal's triangle, each number is the sum of the two numbers directly above it.
<!-- more -->

**Example:**
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## Analysis
&emsp;&emsp;这是一道很经典的**杨辉三角**题目，题目要求我们计算出杨辉三角。一个很自然的想法就是逐层计算，不断利用上一层的信息来计算当前层的内容。
&emsp;&emsp;首先我们需要构建好第一层，第一层只含一个`1`。然后我们就可以使用循环来构建下面的层。每一层的第一个元素和最后一个元素都是`1`这个需要我们手动添加进去。然后从下标`i = 1`开始，利用上层的`i - 1`和`i`计算当前层的第`i`个元素。

## Solution
&emsp;&emsp;按照上面的分析编写代码，注意`numRows`为0的情况。同时，注意循环中使用上一层元素时的边界问题，`j`从`i`到`i-1`。

## Code
```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result;
        if (numRows == 0) {
            return result;
        }

        // Initialize first row
        vector<int> v;
        v.emplace_back(1);
        result.emplace_back(v);


        for (int i = 1; i < numRows; i++) {
            vector<int> temp;
            temp.emplace_back(1);

            for (int j = 1; j <=i-1; j++) {
                int x = result[i-1][j-1] + result[i-1][j];
                temp.emplace_back(x);
            }

            temp.emplace_back(1);
            result.emplace_back(temp);
        }
        return result;
    }
};
```
**运行时间：**约0ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;这道题是很经典的杨辉三角，考察的主要是循环语句的使用。做之前需要分析每层的特点。这里考虑到每层头和尾的`1`是要自己插入的，中间元素则是利用上一层的元素来计算，同时需要注意越界问题，可以说要完全做对还是需要仔细的思考。本题的分析到这里，谢谢您的支持！
