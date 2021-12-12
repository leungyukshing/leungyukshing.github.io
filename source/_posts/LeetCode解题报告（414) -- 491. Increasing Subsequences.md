---
title: LeetCode解题报告（414) -- 491. Increasing Subsequences
mathjax: true
date: 2021-12-12 14:38:37
---

## Problem

Given an integer array `nums`, return all the different possible increasing subsequences of the given array with **at least two elements**. You may return the answer in **any order**.

The given array may contain duplicates, and two equal integers should also be considered a special case of increasing sequence.

<!-- more -->

**Example 1:**

```
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**Example 2:**

```
Input: nums = [4,4,3,2,1]
Output: [[4,4]]
```

**Constraints:**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求找出所有递增的子数组，并且数组的大小必须大于1。题目中说到要求返回所有的可能的结果，这就是一个很明显的提示要使用backtracking。

&emsp;&emsp;基于数组的backtracking是有套路的，参数包括数组本身`nums`，一个下标`idx`表示当前处理到哪个位置的数字，还有一个`current`数组表示当前的结果，以及返回的答案`result`。这四个元素基本就是数组类回溯中不可或缺的四要素。然后我们来看什么情况下要加入到答案，这部分的代码要写在函数的开头。因为所有大小大于1的都是答案，所以只需要判断`current.size()`大于1就可以加入到答案了。同时，当`idx == nums.size()`时，表明已经遍历完数组，也是可以直接return。

&emsp;&emsp;接着我们来看怎么处理回溯的部分，这里也是套路，一个`for`循环从`idx`开始遍历到最后一个位置，然后是选择当前元素，调用函数，最后再撤回操作。在选择当前元素时，因为要**保证非递减**，所以要保证`current.back() <= nums[i]`，当然如果`current`为空也可以选择当前元素。

&emsp;&emsp;最后就是处理special case。这道题目中的special case是，当数组中有相邻重复的元素时，答案只需要其中一个。比如example1中的`[4,6,7]`，其中7有两个，但选择第一个7和选择第二个7的效果是一样的，所以这里要进行去重。同时又要保证`[7,7]`这个也是一种答案。因此，不能够在一开始对原始的数组进行去重，而是要在每次选择的时候，进行去重。比如当处理到`[4,6]`后，下一个元素只要选择过7了，就不需要再选择7了，所以这个去重是在回溯函数中的。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<int> current;
        vector<vector<int>> result;
        helper(nums, 0, current, result);
        return result;
    }
private:
    void helper(vector<int>& nums, int idx, vector<int> current, vector<vector<int>> &result) {
        if (current.size() > 1) {
            result.push_back(current);                
        }
        
        if(idx == nums.size()) {
            return;
        }
        unordered_set<int> st;
        for (int i = idx; i < nums.size(); ++i) {
            if ((current.empty() || current.back() <= nums[i]) && st.count(nums[i]) == 0) {
                current.push_back(nums[i]);
                helper(nums, i + 1, current, result);
                current.pop_back();
                st.insert(nums[i]);
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这是一道较为简单的数组类型的回溯问题，主要难点在于元素的去重，其他部分都是套路。这道题目的分享到这里，感谢你的支持！

