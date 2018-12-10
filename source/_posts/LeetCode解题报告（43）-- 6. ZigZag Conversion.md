---
title: LeetCode解题报告（43）-- 6. ZigZag Conversion
tags:
  - LeetCode
abbrlink: 61068
date: 2018-12-10 12:20:54
---
## Problem
The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: `"PAHNAPLSIIGYIR"`
Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string s, int numRows);
```
<!-- more -->

**Example 1:**
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

## Analysis
&emsp;&emsp;这道题的考点是字符串的操作，我们需要对一个字符串用`N`型的方式去放置这个字符串。其本质是对矩阵的操作，操作的方向有两个，一个是往上，另一个是往下。根据题目给的`numRows`，确定遍历的界限即可。

## Solution
&emsp;&emsp;使用一个变量`index`来访问不同行，使用`dir`来表示当前的移动方向（1为向下，-1为向上）。然后遍历字符串，每个字符放到对应的`zig[index]`中就可以了。需要注意边界情况：
  + 当向上走时，`index < 0`时，说明此时已经到最上一行，需要将`dir`设为1，`index`设为1；
  + 同理，当向下走时，`index >= numRows`时，说明此时已经走到了最下一行，需要将`dir`设为-1，`index`设为`numRows - 2`
&emsp;&emsp;最后再将所有的行拼接起来成为一个字符串作为结果输出即可。

## Code
```C++
class Solution {
public:
    string convert(string s, int numRows) {
        if (s.size() < numRows || numRows == 1)
            return s;

        int dir = 1;// 1 go down, -1 go up
        int index = 0; // pos in zig
        string zig[numRows];
        for (int i = 0; i < s.size(); i++) {
            zig[index] += s[i];
            index += dir;

            if(index < 0) {
                index = 1;
                dir = 1;
            }
            if (index >= numRows) {
                index = numRows - 2;
                dir = -1;
            }
        }

        string result;
        for (int i = 0; i < numRows; i++) {
            if (!zig[i].empty()) {
                result += zig[i];
            }
        }
        return result;
    }
};
```
**运行时间：**约20ms，超过75.6%的CPP代码。

## Summary
&emsp;&emsp;这道题对算法没什么要求，其实就是一个遍历矩阵，需要耐心地找规律。网上也有一种很巧妙的解法可以[参考](https://blog.csdn.net/gatieme/article/details/50889937)，但是像我这样解决也是比较快速的。本题的分析到此为止，谢谢您的支持。
