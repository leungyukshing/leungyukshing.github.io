---
title: LeetCode解题报告（二十）-- 136. Single Number
tags:
  - LeetCode
abbrlink: 42647
date: 2018-10-16 00:38:06
---
## Problem
Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
<!-- more -->

**Example 1**:
```
Input: [2,2,1]
Output: 1
```

**Example 2**:
```
Input: [4,1,2,1,2]
Output: 4
```

## Analysis
&emsp;&emsp;这道题的意思很简单，就是找出数组中只出现一次的数字。方法当然有很多种：可以结合map遍历来标记重复出现的数字，也可以暴力两重循环。当然我们的目的不只是解决这道题，而是要使用一种**线性复杂度**的方法。这里我们充分利用**异或运算**来实现，这也是目前而言最快的算法了。
&emsp;&emsp;异或运算有几个很好的特点：
  + 具有交换律和结合律
  + `x^x = 0`
  + `x^0 = x`
&emsp;&emsp;然后我们可以利用上述三个特性简要地证明一下：对于$$T=1^2^...^n^...^n^...^1000（含有两个相同的n）$$，可以转化为
$$
T = 1^2^...^1000^(n^n)
T = 1^2^...^1000^0
T = 1^2^...^1000
$$
经过上述推导，我们可以得出如下结论：对于连续的数进行异或运算，结果是只出现一次的数字的异或结果。而本题给出的输入中，重复的数字只会出现两次！这样我们就可以直接使用这个结论了。

## Solution
&emsp;&emsp;对数组中的每个数进行异或操作，最后的结果就是**只出现一次**的数了。

## Code
```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for (int i = 0; i < nums.size(); i++) {
            result ^= nums[i];
        }
        return result;
    }
};
```
**运行时间：**约8ms，超过98.79%的CPP代码。
## Summary
&emsp;&emsp;这道题难度在于如何找到线性的方法，使用异或来求解是一种新颖而快速的思路，对于日后处理众多类似的问题很有意义，因此我觉得这题很有价值！谢谢大家的支持!
