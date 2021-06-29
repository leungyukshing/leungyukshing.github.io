---
title: LeetCode解题报告（233）-- 382. Linked List Random Node
tags:
  - LeetCode
mathjax: true
abbrlink: 61194
date: 2020-12-02 17:31:48
---

## Problem

Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability** of being chosen.

**Follow up:**
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

<!-- more -->

**Example:**

```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
```

------

## Analysis

&emsp;&emsp;这道题目的意思非常简单，给定一个链表，要求我们在这个链表中随机抽取一个数字出来，要保证这个抽取是等概率的。最简单的做法就是和数组一样，首先计算好整个链表的长度，然后每次抽取的时候随机一个数字，对长度取余，这样每个数字被抽到的概率就是相等的。但是这样有一个问题，我们需要在初始化的时候就计算出这个链表的长度，然后每次抽取的时候又需要遍历到抽到的那个数字。能不能有一种更加快速的方法，只需要一次遍历就能做到呢？

&emsp;&emsp;这里会用到**水塘抽样**的随机算法，在下面reference有详细的介绍，这里我只结合题目做简单的解释。水塘抽样的背景是**从未知大小的数据流中选取k个数据，保证每个数据被抽到的概率相等**。在这道题目`k=1`，所以我们只需要考虑这种情况。假设一共有`N`个数，那么到最后我们要求的是每个数字被选中的概率都是$\frac{1}{N}$，抽取的过程如下：

- 遇到第一个数$n_1$，直接保留，$p(n_1) = 1$；
- 遇到第二个数$n_2$， 以$\frac{1}{2}$的概率保留，那么$p(n_1) = 1 \times \frac{1}{2}$，$p(n_2) = \frac{1}{2}$，这里$p(n_1)$的意思是在第一个数的时候选中但是第二个数的时候没选到；
- 遇到第三个数$n_3$，以$\frac{1}{3}$的概率保留，那么$p(n_1) = p(n_2) = \frac{1}{2} \times (1 - \frac{1}{3}) = \frac{1}{3}$，$p(n_3) = \frac{1}{3}$，这里$p(n_1)$的意思是在第二个数的时候选中但是第三个数的时候没选到；
- 以此类推，遇到第`i`个数$n_i$的时候，以$\frac{1}{i}$的概率保留，那么$p(n_1)= p(n_2) = \dots = p(n_{i-1}) = \frac{1}{i-1} \times (1 - \frac{1}{i})$， $p(n_i) = \frac{1}{i}$。

&emsp;&emsp;这个算法的关键思路就是每个数字在前面被选中之后，因为后面不断是其他数组被选中，所以它被选中的概率不断地减少，这样就能够保证所有数字是等概率被选到的。

------

## Solution

&emsp;&emsp;根据上面的递归公式直接coding即可。

------

## Code

初级版：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        this->head = head;
        this->size = 0;
        while (head != NULL) {
            head = head->next;
            this->size++;
        }
        // cout << this->size << endl;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        int idx = (rand() % this->size);
        // cout << idx << endl;
        ListNode* p = this->head;
        while (idx != 0) {
            p = p->next;
            idx--;
        }
        return p->val;
    }
private:
    ListNode* head;
    int size;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

高级版：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        this->head = head;
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        int result = head->val;
        int i = 2;
        ListNode* p = head->next;
        while (p) {
            int prob = rand() % i;
            if (prob == 0) {
                result = p->val;
            }
            i++;
            p = p->next;
        }
        return result;
    }
private:
    ListNode* head;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

------

## Summary

&emsp;&emsp;这道题目本身不难，但是如果要减少为一次遍历就有点难度了。借助这道题目，我还了解了水塘抽样这个随机算法，自己推导一下发现还是很巧妙的。这道题目也具有很强的实用背景，在很多业务场景，内存空间有限的前提下，我们未必能够知道数据的总长度，这个时候使用水塘抽样就非常简便。这道题目的分享到这里，谢谢您的支持！

------

## Reference

1. [水塘抽样（Reservior Sampling）](https://zhuanlan.zhihu.com/p/29178293)
2. [水塘抽样 wikipedia](
