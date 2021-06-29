---
title: LeetCode解题报告（311)-- 706. Design HashMap
tags:
  - LeetCode
mathjax: true
abbrlink: 53325
date: 2021-03-09 22:51:03
---

## Problem

Design a HashMap without using any built-in hash table libraries.

To be specific, your design should include these functions:

- `put(key, value)` : Insert a (key, value) pair into the HashMap. If the value already exists in the HashMap, update the value.
- `get(key)`: Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
- `remove(key)` : Remove the mapping for the value key if this map contains the mapping for the key.

<!-- more -->

**Example:**

```
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // returns 1
hashMap.get(3);            // returns -1 (not found)
hashMap.put(2, 1);          // update the existing value
hashMap.get(2);            // returns 1 
hashMap.remove(2);          // remove the mapping for 2
hashMap.get(2);            // returns -1 (not found) 
```

**Note:**

- All keys and values will be in the range of `[0, 1000000]`.
- The number of operations will be in the range of `[1, 10000]`.
- Please do not use the built-in HashMap library.

------

## Analysis

&emsp;&emsp;题目要求我们设计一个hashmap，要求的操作有`get`、`put`和`remove`。题目不允许使用map的数据结构，但是可以使用其他的数据结构。因为key的范围是`[0, 1000000]`，所以我们不妨直接开一个1000000的数组来存放，key就是下标，value就是对应的值。

&emsp;&emsp;因为这里要区分key存在和不存在的问题，所以要用-1标记不存在的key。数组初始化的时候就是-1，`remove`操作后也是-1。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class MyHashMap {
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        // init array
        for (int i = 0; i < 1000001; i++) {
            this->storage[i] = -1;
        }
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        this->storage[key] = value;
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        return this->storage[key];
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        this->storage[key] = -1;
    }
private:
    int storage[1000001];
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

------

## Summary

&emsp;&emsp;这道题目其实就是用一个数组实现一个hashmap，难度不大，但是在一些基础的面试题会遇到。这道题目的分享到这里，感谢你的支持！
