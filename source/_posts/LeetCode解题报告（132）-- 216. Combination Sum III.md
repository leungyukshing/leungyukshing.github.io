---
title: LeetCode解题报告（132）-- 216. Combination Sum III
tags:
  - LeetCode
mathjax: true
date: 2020-09-12 22:25:27

---

## Problem

Find all possible combinations of ***k*** numbers that add up to a number ***n***, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

<!-- more -->

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

------

## Analysis

&emsp;&emsp;这道题目要求我们用`k`个数字相加得到某个给定的值`n`，数字选择的范围是`1-9`，要返回所有的组合。同时还要求一个数字最多只能出现一次，所以再每次选择的时候不能考虑之前已经使用过的数字了。

&emsp;&emsp;这道题目的做法毫无疑问是递归，因为父问题和子问题是一样的，以example1为例，当第一次选择了1之后，那么问题就变为`k=2`和`n=6`了，这就是很明显使用递归的特征。

&emsp;&emsp;选择好大致的方法后，就来看一下每个递归内要做什么了，首先是上面提到的数字选择的范围，因为要保证唯一性，所以我们干脆就从1开始选，选完之后下一个递归就不能选了，这个信息需要在递归中传递。然后就是要解决example2中`[1,2,6]`和`[1,3,5]`这种情况，在考虑第二种情况的时候，选了1一直需要跳过2才能选到3，怎么实现这种呢？实际上这个就有点DFS的味道了，开始递归就是DFS，然后完成一个之后就把刚刚选中的取消掉，换下一个。

&emsp;&emsp;判断是否结束也很容易，如果`n < 0`说明减成负数了，该组合的数字之和就不是目标数字，如果`n = 0 && temp.size() == k`，说明刚好k个数字之和是n，这个就是正确答案了。

------

## Solution

&emsp;&emsp;递归的时候要带上可选数字的范围信息（start）和正在计算的组合，当然还有总的答案。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> result;
        vector<int> temp;
        dfs(k, n, 1, temp, result);
        return result;
    }
private:
    void dfs(int k, int n, int start, vector<int>& temp, vector<vector<int>>& result) {
        if (n < 0) {
            return;
        }
        if (n == 0 && temp.size() == k) {
            result.push_back(temp);
            return;
        }
        for (int i = start; i < 10; i++) {
            temp.push_back(i);
            dfs(k, n - i, i + 1, temp, result);
            temp.pop_back();
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目首先要看得出是用递归，要找出父问题如何变成子问题的。然后是考虑多种组合的情况，再根据这些情况看看递归过程中需要什么信息，在递归的时候带上就可以了。这道题的分析到这里，谢谢！
