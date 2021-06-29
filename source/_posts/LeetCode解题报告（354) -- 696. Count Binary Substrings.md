---
title: LeetCode解题报告（354) -- 696. Count Binary Substrings
tags:
  - LeetCode
mathjax: true
abbrlink: 34641
date: 2021-04-27 23:04:22
---

## Problem

Give a string `s`, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

<!-- more -->

**Example 1:**

```
Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.
```

**Example 2:**

```
Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
```

**Note:**

- `s.length` will be between 1 and 50,000.

- `s` will only consist of "0" or "1" characters.

------

## Analysis

&emsp;&emsp;题目给出一个01串，要求找出符合要求的子串的个数。子串的要求有：

1. 子串中0和1的数量要相等；
2. 子串中0和1要各自连续。

&emsp;&emsp;第一个要求好理解，第二个要求有点隐晦。各自连续的意思是，如果是`0011`则满足要求，而`0101`则不满足，因为两个0和两个1都不连在一起。有了第二个要求实际上对解题是有帮助的，因为只要检测到不连续，01交替或者10交替，说明要开始新子串的检测了。

&emsp;&emsp;具体的做法如下，我们用两个值分别记录两种数字出现的次数，但我们并不指定哪个值统计的0，哪个值统计的是1，两个值交替使用即可。以example1为例，我们初始化两个值`pre = 0`，`cur = 1`，这个`cur`的实际意义是默认统计第一个出现的字符的个数，在`"00110011"`中指的就是0的个数。然后向后遍历，当遇到01转换时，把`cur`的值赋给`pre`，此时`pre`代表的是0的个数，而`cur`代表的是1的个数。继续往后遍历，当遇到10转换时，再次把`cur`赋给`pre`，此时`pre`代表的是1的个数，`cur`代表的是0的个数，以此循环。

&emsp;&emsp;转换的关键节点在上面已经描述清楚，那么`pre`和`cur`实际上就是两个不同字符出现的次数，我们怎么通过次数来判断是否有符合要求的子串的？假设`pre = 0`，`cur = 1`，此时，`pre < cur`，所以无法构成子串；当完成第一个01转换后，`pre = 2`，`cur = 1`，此时`pre > cur`，这就说明，前面的两个0，能分出一个0和后面的1构成符合要求的子串`01`；继续往后，`pre = 2`，`cur = 2`，此时`pre == cur`，这就说明前面的两个0和后面的两个1构成了符合要求的子串`0011`。因此判断是否有符合要求的子串的条件就是`pre >= cur`。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int pre = 0, cur = 1;
        int size = s.size();
        int result = 0;
        for (int i = 1; i < size; i++) {
            if (s[i - 1] != s[i]) {
                pre = cur;
                cur = 1;
            } else {
                cur++;
            }
            if (pre >= cur) {
                result++;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是基于字符串的一个逻辑题，编码上没有难度，难点在于逻辑思考。很巧妙地使用两个变量交替计数，然后利用数量的大小关系来统计符合要求的子串个数。这道题目的分享到这里，感谢你的支持！
