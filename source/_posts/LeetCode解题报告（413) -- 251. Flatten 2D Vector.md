---
title: LeetCode解题报告（413) -- 251. Flatten 2D Vector
mathjax: true
date: 2021-12-11 21:56:25
---

## Problem

Design an iterator to flatten a 2D vector. It should support the `next` and `hasNext` operations.

<!-- more -->

Implement the `Vector2D` class:

- `Vector2D(int[][] vec)` initializes the object with the 2D vector `vec`.
- `next()` returns the next element from the 2D vector and moves the pointer one step forward. You may assume that all the calls to `next` are valid.
- `hasNext()` returns `true` if there are still some elements in the vector, and `false` otherwise.

**Example 1:**

```
Input
["Vector2D", "next", "next", "next", "hasNext", "hasNext", "next", "hasNext"]
[[[[1, 2], [3], [4]]], [], [], [], [], [], [], []]
Output
[null, 1, 2, 3, true, true, 4, false]

Explanation
Vector2D vector2D = new Vector2D([[1, 2], [3], [4]]);
vector2D.next();    // return 1
vector2D.next();    // return 2
vector2D.next();    // return 3
vector2D.hasNext(); // return True
vector2D.hasNext(); // return True
vector2D.next();    // return 4
vector2D.hasNext(); // return False
```

**Constraints:**

- `0 <= vec.length <= 200`
- `0 <= vec[i].length <= 500`
- `-500 <= vec[i][j] <= 500`
- At most `105` calls will be made to `next` and `hasNext`.

**Follow up:** As an added challenge, try to code it using only [iterators in C++](http://www.cplusplus.com/reference/iterator/iterator/) or [iterators in Java](http://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html).

------

## Analysis

### Method 1

&emsp;&emsp;这是一道数据结构设计题，整个框架非常典型，也是一个嵌套的结构实现一个iterator，其中包括`next()`和`hasNext()`两个方法。这类题目一般有两种解法，第一种是在constructor中就把所以的数据都打平，存成一维的结构；第二种是使用一些变量去记录当前访问到哪个位置。第一种解法通常是不好的，因为对于infinite的数据来说是不可行，一般面试中也不会用第一种。所以这里我们就focus在第二种方法。

&emsp;&emsp;首先来看看题目，这道题目要打平的数据是二维数组，非常好处理。实际上对于任意一个元素，我们需要知道两个信息就能访问，一个是它所在的vector的Idx，也就是`vectorIdx`，另一个是它在自己vector中的idx，也就是`eleIdx`，于是我们只需要记录这两个变量即可。两个下标表示的是`next()`函数应该返回的数字。在往后移动的时候，需要跳过空的vector，所以`hasNext()`的判断依据就是`vectorIdx`是否等于vector的数量。

### Method 2

&emsp;&emsp;上面的方法虽然只用了两个信息，但是需要把整个二维数组存下来。还有一种方法是只通过迭代器。使用迭代器的方法同样需要记录两个信息，一个是外层的迭代器，一个是内层的迭代器。同时，因为我们不再存储整个二维数组，所以还需要记录一下外层的`end`迭代器用于判断结束。

&emsp;&emsp;至于`next()`更新的逻辑和上面的方法类似，无非就是从下标操作转为迭代器的操作。

## Solution

&emsp;&emsp;无。

------

## Code

### Method 1

```c++
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec) {
        v = vec;
        vectorIdx = 0;
        eleIdx = 0;
        while (vectorIdx < v.size() && v[vectorIdx].size() == 0) {
            vectorIdx++;
        }
    }
    
    int next() {
        int result = v[vectorIdx][eleIdx];
        if (eleIdx < v[vectorIdx].size() - 1) {
            eleIdx++;
        } else if (eleIdx == v[vectorIdx].size() - 1) {
            vectorIdx++;
            while (vectorIdx < v.size() && v[vectorIdx].size() == 0) {
                vectorIdx++;
            }
            eleIdx = 0;
        }
        return result;
    }
    
    bool hasNext() {
        return vectorIdx != v.size();
    }
private:
    int vectorIdx, eleIdx;
    vector<vector<int>> v;
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D* obj = new Vector2D(vec);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

### Method 2

```c++
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec) {
        outerIter = vec.begin();
        outerEnd = vec.end();
        while (outerIter != outerEnd && outerIter->size() == 0) {
            outerIter++;
        }
        if (outerIter == outerEnd) {
            return;
        }
        innerIter = outerIter->begin();
    }
    
    int next() {
        int result = *innerIter++;
        while (outerIter != outerEnd && innerIter == outerIter->end()) {
            outerIter++;
            if (outerIter == outerEnd) {
                break;
            }
            innerIter = outerIter->begin();
        }
        return result;
    }
    
    bool hasNext() {
        return outerIter != outerEnd;
    }
private:
    vector<vector<int>>::iterator outerIter;
    vector<vector<int>>::iterator outerEnd;
    vector<int>::iterator innerIter;
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D* obj = new Vector2D(vec);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

------

## Summary

&emsp;&emsp;这是一道比较常规的数据结构设计题，因为需要打平的数据是二维数组，所以本质上还是记录下标来实现。这道题目的分享到这里，感谢你的支持！

