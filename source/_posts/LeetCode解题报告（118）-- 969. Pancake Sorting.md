---
title: LeetCode解题报告（118）-- 969. Pancake Sorting
tags:
  - LeetCode
mathjax: true
date: 2020-08-30 01:43:11


---

## Problem

Given an array of integers `A`, We need to sort the array performing a series of **pancake flips**.

In one pancake flip we do the following steps:

- Choose an integer `k` where `0 <= k < A.length`.
- Reverse the sub-array `A[0...k]`.

For example, if `A = [3,2,1,4]` and we performed a pancake flip choosing `k = 2`, we reverse the sub-array `[3,2,1]`, so `A = [**1,2,3**,4]` after the pancake flip at `k = 2`.

Return *an array of the k-values of the pancake flips* that should be performed in order to sort `A`. Any valid answer that sorts the array within `10 * A.length` flips will be judged as correct.

<!-- more -->

**Example 1:**

```
Input: A = [3,2,4,1]
Output: [4,2,4,3]
Explanation: 
We perform 4 pancake flips, with k values 4, 2, 4, and 3.
Starting state: A = [3, 2, 4, 1]
After 1st flip (k = 4): A = [1, 4, 2, 3]
After 2nd flip (k = 2): A = [4, 1, 2, 3]
After 3rd flip (k = 4): A = [3, 2, 1, 4]
After 4th flip (k = 3): A = [1, 2, 3, 4], which is sorted.
Notice that we return an array of the chosen k values of the pancake flips.
```

**Example 2:**

```
Input: A = [1,2,3]
Output: []
Explanation: The input is already sorted, so there is no need to flip anything.
Note that other answers, such as [3, 3], would also be accepted.
```

**Constraints:**

- `1 <= A.length <= 100`
- `1 <= A[i] <= A.length`
- All integers in `A` are unique (i.e. `A` is a permutation of the integers from `1` to `A.length`).

------

## Analysis

&emsp;&emsp;这是一道与字符串翻转有关的题目。题目定义一个flip操作是从头开始到指定下标的翻转，题目要求通过若干个flip操作，对数组进行排序。flip操作本身是不能带来有序性的，注意到题目给的条件是，数组中的元素范围刚好是和size的范围一样的，所以我们干脆用笨一点的方法，一次排好一个位置。因为排序后，值为`k`的元素应该在下标为`k-1`的位置上。我们可以先从数值最大的开始（下标最大），因为排好之后，就不需要移动了，flip前面的就可以了。

&emsp;&emsp;问题就转化为如何把一个数`k`放到对应的位置`k-1`上了。其实flip两次就可以了，第一次把这个数flip到第一个，第二次把第`k-1`前面的flip，这样`k`就会放在了`k-1`这个位置上了。

------

## Solution

&emsp;&emsp;为了方便起见可以实现一个flip函数，每次flip都记录下flip的位置，然后记录到答案中。特别注意，如果元素本身就在对应的位置了，可以skip掉flip，这样可以节省flip的次数。

------

## Code

```c++
class Solution {
public:
    vector<int> pancakeSort(vector<int>& A) {
        int size = A.size();
        
        for (int i = size; i >= 1; i--) {
            int index = 0;
            while (A[index] != i && index < size) {
                index++;
            }
            if (index == i - 1) {
                continue;
            }
            // cout << index << " " << i-1 << endl;
            flip(index, A);
            flip(i - 1, A);
        }
        return result;
    }
    
    void flip(int index, vector<int>& A) {
        // cout << "flip " << index << endl;
        for (int i = 0; i <= index / 2; i++) {
            // cout << "swap " << i << " " << index - i << endl;
            swap(A[i], A[index - i]);
        }
        result.push_back(index + 1);
    }
private:
    vector<int> result;
};
```

------

## Summary

&emsp;&emsp;这道题考查了两个点，第一个是字符串的翻转，对于大多数人来说应该都没什么难度；第二个就是这种数组元素的范围和长度一致的题目，这类题目可以利用的一个性质是：有序情况下每个元素的下标都是固定的。这道题的分析到这里，谢谢！
