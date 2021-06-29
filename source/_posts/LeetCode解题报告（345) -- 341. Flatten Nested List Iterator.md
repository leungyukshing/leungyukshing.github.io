---
title: LeetCode解题报告（345) -- 341. Flatten Nested List Iterator
tags:
  - LeetCode
mathjax: true
abbrlink: 53343
date: 2021-04-18 22:24:02
---

## Problem

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the `NestedIterator` class:

- `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list `nestedList`.
- `int next()` Returns the next integer in the nested list.
- `boolean hasNext()` Returns `true` if there are still some integers in the nested list and `false` otherwise.

<!-- more -->

**Example 1:**

```
Input: nestedList = [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
```

**Example 2:**

```
Input: nestedList = [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].
```

**Constraints:**

- `1 <= nestedList.length <= 500`
- The values of the integers in the nested list is in the range `[-106, 106]`.

------

## Analysis

&emsp;&emsp;题目给出了一个嵌套链表，要求我们实现一个迭代器去遍历这个嵌套链表。链表中的元素可以是数字，也可以是链表，每个元素的类型提供接口判断。

&emsp;&emsp;一开始的想法是维护一个下标，指向当前已经遍历到那个位置了，这个思路对普通链表来说是没问题的，但是对于嵌套链表，无法快速定位到当前指向的位置。既然在使用过程中我们无法做到，干脆初始化的时候把嵌套链表打平就好，然后按照数组一个一个往后读。

&emsp;&emsp;所以这道题目就转变为打平一个嵌套链表，其实就是DFS了。把元素都提取出来放到一个数组中，然后维护一个下标表示当前访问到哪个位置即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        cursor = 0;
        int size = nestedList.size();
        for (int i = 0; i < size; i++) {
            helper(arr, nestedList[i]);
        }
        
        for (int i = 0; i < arr.size(); i++) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }
    
    int next() {
        return arr[cursor++];
    }
    
    bool hasNext() {
        return cursor < arr.size();
    }
private:
    vector<int> arr;
    int cursor;
    void helper(vector<int> &result, NestedInteger integer) {
        if (integer.isInteger()) {
            result.push_back(integer.getInteger());
        } else {
            vector<NestedInteger> list = integer.getList();
            int size = list.size();
            for (int i = 0; i < size; i++) {
                helper(result, list[i]);                
            }
        }
        // return result;
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

------

## Summary

&emsp;&emsp;这道题目难度不大，是数组中一个简单的DFS。这道题目的分享到这里，感谢你的支持！
