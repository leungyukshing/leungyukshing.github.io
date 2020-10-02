---
title: LeetCode解题报告（177）-- 39. Combination Sum
tags:
  - LeetCode
mathjax: true
date: 2020-10-02 19:38:30

---

## Problem

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

<!-- more -->

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Example 4:**

```
Input: candidates = [1], target = 1
Output: [[1]]
```

**Example 5:**

```
Input: candidates = [1], target = 2
Output: [[1,1]]
```

**Constraints:**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 500`

------

## Analysis

&emsp;&emsp;这道题目是非常典型的找到所有答案的题目，处理这类题目已经有比较成熟的套路了，无非就是通过递归进行BFS的操作。递归过程中带上必要的信息。这道题是求和的，所以我们每选到一个数字，`target`就减去相应的值，如果`target`减到0，则说明成功找到一个组合了。

------

## Solution

&emsp;&emsp;递归的过程需要携带的信息有：

1. `candidates`：题目给出的备用数字；
2. `target`：需要在递归中不断更新，同时要通过判断是否为0来找到答案；
3. `start`：在`candidates`中开始搜索的下标，为了避免重复，这个是必须的；
4. `present`：当前找的数字集合；
5. `result`：存储答案。

&emsp;&emsp;需要特别注意的是，因为题目允许重复的数字出现多次，所以每次遍历都要从`start`开始，而不是`start + 1`。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> temp;
        helper(candidates, target, 0, temp, result);
        return result;
    }
private:
    void helper(vector<int>& candidates, int target, int start, vector<int>& present, vector<vector<int>>& result) {
        if (target < 0) {
            return;
        } else if (target == 0) {
            result.push_back(present);
            return;
        } else {
            int size = candidates.size();
            for (int i = start; i < size; i++) {
                present.push_back(candidates[i]);
                helper(candidates, target - candidates[i], i, present, result);
                present.pop_back();
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;前面做过了很多类似的题目，所以现在面对这种题目思路一下子就出来了，coding方面也有成熟的套路了。因此对题目进行总结能够很好地增加对题目的熟悉程度，从而提高我们的解题速度。这道题的分析到这里，谢谢！
