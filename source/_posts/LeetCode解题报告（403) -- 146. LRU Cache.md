---
title: LeetCode解题报告（403) -- 146. LRU Cache
mathjax: true
abbrlink: 40702
date: 2021-09-08 11:44:16
---

## Problem

Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

<!-- more -->

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 104`
- `0 <= value <= 105`
- At most 2` * 105` calls will be made to `get` and `put`.

------

## Analysis

&emsp;&emsp;这题要求实现一个LRU Cache，有一些基本的要求。首先是`get`和`put`的时间复杂度都要求是$O(1)$，所以这里很明显就要用到hashmap了。然后LRU一个最主要的特点就是，当缓存满了之后，需要淘汰到最少使用的一个key，因此这里我选用了list，主要是维护一个顺序。

&emsp;&emsp;list的选用比较简单，只需要记录一个`pair<int,int>`，包含key和value；hashmap的选用就需要仔细思考了，因为要通过key获取到，所以key肯定就是用户传入的key，但是value就不能是用户传入的value。因为在用户访问key的时候，需要调整list的顺序，所以这里要存的是iterator，可以直接访问到list中相对应的位置。

&emsp;&emsp;对`get`方法，先在hashmap中查询key是否存在，如果不存在直接返回-1，如果存在的话就返回value，**同时利用iterator调整在list中的位置**，把对应的value调整到list中第一个位置。

&emsp;&emsp;对于`put`方法，先在hashmap中查询key是否存在，如果存在则先从list中删掉，然后统一添加到队头。此时需要更新hashmap中的迭代器，因为是插入到队头，所以只需要去第一个元素的迭代器即可。这里还需要处理超出cache大小的情况，因为要淘汰掉list末尾的元素，所以只需要删除掉最后一个元素，并且在hashmap中删除对应的key即可。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class LRUCache {
public:
    LRUCache(int capacity) {
        cap = capacity;
    }
    
    int get(int key) {
        auto iter = m.find(key);
        if (iter == m.end()) {
            return -1;
        }
        l.splice(l.begin(), l, iter->second);
        return iter->second->second;
    }
    
    void put(int key, int value) {
        auto iter = m.find(key);
        if (iter != m.end()) {
            l.erase(iter->second);
        }
        // push to the front of list
        l.push_front(make_pair(key, value));
        m[key] = l.begin();
        // remove last one if exceed the limit
        if (l.size() > cap) {
            int k = l.rbegin()->first;
            l.pop_back();
            m.erase(k); // delete according to key
        }
    }
private:
    int cap;
    list<pair<int, int>> l;
    map<int, list<pair<int, int>>::iterator> m;
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

------

## Summary

&emsp;&emsp;这道题目是一道非常经典的面试题，首先LRU本身就是一个很基础的算法，同时在实现过程中考察到了list、hashmap的使用。这道题目的分享到这里，感谢你的支持！
