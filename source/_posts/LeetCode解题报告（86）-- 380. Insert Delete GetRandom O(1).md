---
title: LeetCode解题报告（86）-- 380. Insert Delete GetRandom O(1)
tags:
  - LeetCode
mathjax: true
abbrlink: 33065
date: 2020-06-12 19:20:12
---

## Problem

Design a data structure that supports all following operations in *average* **O(1)** time.

1. `insert(val)`: Inserts an item val to the set if not already present.
2. `remove(val)`: Removes an item val from the set if present.
3. `getRandom`: Returns a random element from current set of elements. Each element must have the **same probability** of being returned.

<!-- more -->

**Example 1:**

```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

------

## Analysis

&emsp;&emsp;这道题目本身非常简单，就是一个数组的增删查。但是题目给出了复杂度限制是$O(1)$，这就增加了难度了。对于三个操作我们可以分开来看：

- 增：插入操作本身的复杂度是$O(1)$，对于数组来说只要在后面加一个元素就可以了。但是题目要求的时候需要在插入的时候判断元素是否存在，这就需要一个查询操作，这个仅靠数组是无法实现的。
- 查：题目要求的是随机查询，所以只需要随机生成数组的下标，直接访问就可以了，这个复杂度也是$O(1)$。
- 删：删除是比较复杂的，和插入一样首先需要定位到元素（找到下标）。删除之后还需要移动其他的元素。

&emsp;&emsp;经过上面三个操作的分析，我们知道除了要使用一个数组来存放元素本身，还需要使用另外一个数据结构去帮助我们检索。考虑到复杂度，很自然地就会想到用`map`。

------

## Solution

&emsp;&emsp;我们一共需要两个数据结构，数组用`vector`，`map`就直接用C++提供的就好。其中`map`存放的key是`val`本身，value是在数组中的下标，这样就可以通过值来找到下标，实现$O(1)$查询。在这种设计下，插入和删除的时候查询元素是否存在的问题就解决了。

&emsp;&emsp;重点来看删除后数组的管理。删除一个元素后，我们不可能把其余的数据都往前移，因为这样复杂度会很大，所以只能通过交换的方式来做。我们可以把数组中最后一个元素和要被删除元素交换，然后删除最后一个位置，这样操作的时间复杂度就是$O(1)$，这里记得把`map` 中的信息更新。

------

## Code

```c++
class RandomizedSet {
public:
    /** Initialize your data structure here. */
    RandomizedSet() {
        
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        map<int, int>::iterator it = m.find(val);
        if (it != m.end()) {
            return false;
        }
        else {
            m[val] = arr.size();
            arr.push_back(val);
            return true;
        }
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        map<int, int>::iterator it = m.find(val);
        if (it != m.end()) {
            if (arr.size() > 1) {
                int idx = it->second;
                arr[idx] = arr[arr.size() - 1];
            
                m[arr[arr.size() - 1]] = idx;
            }
            m.erase(it);
            arr.pop_back();
            return true;
        } else {
            return false;
        }
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        int idx = random() % arr.size();
        return arr[idx];
    }
private:
    vector<int> arr;
    map<int, int> m;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

------

## Summary

 &emsp;&emsp;这道题目本来是非常简单的，但是加了复杂度限制后，就变得有趣了。如果刷题比较多的人可能一下就能想到解决的方法，但就算是不太熟悉的人，也可以通过分析，首先确定使用的数据结构，再分析每个操作的时间复杂度。在降低复杂度的场景下，`map`和位置交换这两种方式是比较常用的。感谢您的支持，欢迎转发、评论，分享，谢谢！
