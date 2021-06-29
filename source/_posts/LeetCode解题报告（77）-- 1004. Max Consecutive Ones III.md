---
title: LeetCode解题报告（77）-- 1004. Max Consecutive Ones III
tags:
  - LeetCode
mathjax: true
abbrlink: 51101
date: 2019-12-08 13:57:43
---

## Problem

Given an array `A` of 0s and 1s, we may change up to `K` values from 0 to 1.

Return the length of the longest (contiguous) subarray that contains only 1s. 

<!-- more -->

**Example 1:**

```
Input: A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
Output: 6
Explanation: 
[1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1.  The longest subarray is underlined.
```

**Example 2:**

```
Input: A = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], K = 3
Output: 10
Explanation: 
[0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1.  The longest subarray is underlined.
```

**Note:**

1. `1 <= A.length <= 20000`
2. `0 <= K <= A.length`
3. `A[i]` is `0` or `1` 

------

## Analysis

&emsp;&emsp;题目给出一系列的0和1，以及最大的change次数`K`，要求找到最长的连续为1的区间长度。如果没有change，这道题很可能使用DP就直接求解出来，但是这里有了change，而且这个change的次数是不确定的（change次数少于题目给定的K即可），所以我们需要从另外的角度求解。

&emsp;&emsp;对于区间问题，我们常用的方法是双指针，左指针`left`指向区间起始下标，右指针`right`指向区间。这里我们同样使用双指针表示一个区间，同时记录这个区间中0的个数，0的个数表示的是**只要做出change，那么区间长度就是最长的连续1的长度**。当然，我们还需要对0的个数进行限制。

&emsp;&emsp;整个算法就是一个trade-off，我们想尽可能地增加右指针，这样区间长度才会尽可能地大。而我们又需要满足题目对change的要求，因此需要增加左指针，这样才能限制change的数量。

&emsp;&emsp;算法思路为：逐个遍历数组，每次都增加右指针，每遍历一个之后，就检查该区间是否满足题目的要求，如果其中包含0的数量大于题目给定的`K`，我们则移动左指针，缩小区间范围，直至满足条件。

------

## Solution

&emsp;&emsp;记录左指针`left`和右指针`right`，使用变量`zero`记录区间中0的个数，然后每次遍历都更新当前区间的长度，遍历完所有后最长的符合要求的区间长度就被记录下来，也就是答案。

------

## Code

```c++
class Solution {
public:
    int longestOnes(vector<int>& A, int K) {
        int size = A.size();
        int left = 0;
        int right = 0;
        int zero = 0;
        int result = 0;
        
        while (right < size) {
            if (A[right] == 0) {
                zero++;
            }
            while (zero > K) {
                if (A[left] == 0) {
                    zero--;
                }
                left++;
            }
            result = max(result, right - left + 1);
            right++;
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这道题是使用双指针解决数组区间问题的经典例子，我们通过两个指针去维护一个区间，在遍历过程中满足题目的要求，最后得出结果。这道题的思路就和大家分享到这里，欢迎大家评论、转发，谢谢您的支持！