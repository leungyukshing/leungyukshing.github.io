---
title: LeetCode解题报告（336) -- 622. Design Circular Queue
tags:
  - LeetCode
mathjax: true
abbrlink: 25214
date: 2021-04-10 20:16:38
---

## Problem

Design your implementation of the circular queue. The circular queue is a linear data structure in which the operations are performed based on FIFO (First In First Out) principle and the last position is connected back to the first position to make a circle. It is also called "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.

Implementation the `MyCircularQueue` class:

- `MyCircularQueue(k)` Initializes the object with the size of the queue to be `k`.
- `int Front()` Gets the front item from the queue. If the queue is empty, return `-1`.
- `int Rear()` Gets the last item from the queue. If the queue is empty, return `-1`.
- `boolean enQueue(int value)` Inserts an element into the circular queue. Return `true` if the operation is successful.
- `boolean deQueue()` Deletes an element from the circular queue. Return `true` if the operation is successful.
- `boolean isEmpty()` Checks whether the circular queue is empty or not.
- `boolean isFull()` Checks whether the circular queue is full or not.

<!-- more -->

**Example 1:**

```
Input
["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
[[3], [1], [2], [3], [4], [], [], [], [4], []]
Output
[null, true, true, true, false, 3, true, true, true, 4]

Explanation
MyCircularQueue myCircularQueue = new MyCircularQueue(3);
myCircularQueue.enQueue(1); // return True
myCircularQueue.enQueue(2); // return True
myCircularQueue.enQueue(3); // return True
myCircularQueue.enQueue(4); // return False
myCircularQueue.Rear();     // return 3
myCircularQueue.isFull();   // return True
myCircularQueue.deQueue();  // return True
myCircularQueue.enQueue(4); // return True
myCircularQueue.Rear();     // return 4
```

**Constraints:**

- `1 <= k <= 1000`
- `0 <= value <= 1000`
- At most `3000` calls will be made to `enQueue`, `deQueue`, `Front`, `Rear`, `isEmpty`, and `isFull`.

------

## Analysis

&emsp;&emsp;这是一道设计类的题目，要求我们设计一个循环队列。我们还是基于一个数组来实现，循环的实现方式是模运算，这个不是难点，难的是如何维护队首和队尾，以及如何判满和判空。

&emsp;&emsp;先看维护队头和队尾，因为拿头、尾都需要$O(1)$，所以必须记录头尾的下标，调用时能直接获取。这里我把头`front`初始化为`size - 1`，为什么要这样处理？而不是初始化为0或者-1呢？初始化为-1虽然能够表示这个头从来没有用过，但是在代码上不便于统一处理，而需要另外加入逻辑去判断是否为-1，所以不采用这个做法；不采用初始化为0，是因为这个0留给了`rear`。因为`rear`总是需要在`front`的后面，所以当`rear`初始化为0，那么`front`只能在他的前面（就是`size - 1`）。当然这里也可以把`front`初始化为0，把`rear`初始化为1，思路上是没问题的。

&emsp;&emsp;然后来看判满和判空。根据头尾指针去判断太过复杂，这里还牵涉到了循环的问题，不能单纯比较哪个在前哪个在后来决定，所以干脆引入一个新的变量，记录当前队列中有多少元素，直接和队列的容量对比即可。

&emsp;&emsp;接着来看入队和出队的操作。我们`front`和`rear`后初始化，该位置指向的元素就是可以用的，所以，入队时，直接把元素填到`rear`指向的位置，然后`rear`往后移动一位，给下一次入队使用；出队时，`front`往后移动一位。特别注意这里往后移动都需要对数组的size进行模运算，这样才能做到循环。

&emsp;&emsp;最后我们来看怎么样获取队首和队尾。`front`指向的是队头的前一个位置，所以获取队头的时候，直接`front + 1`即可（还是要模运算）。队尾的获取比较复杂，`rear`指向的是队尾的后一个位置，要找到队尾就需要向前一位，但是不能直接`rear - 1`，负数就算不了了，所以这里要先加上长度再模运算，即`rear + size - 1`，然后再模运算。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class MyCircularQueue {
public:
    MyCircularQueue(int k) {
        this->arr = new int[k];
        this->front = k - 1;
        this->rear = 0;
        this->size = k;
        this->count = 0;
    }
    
    bool enQueue(int value) {
        if (isFull()) {
            return false;
        }
        arr[rear] = value;
        rear = (rear + 1) % this->size;
        this->count++;
        return true;
    }
    
    bool deQueue() {
        if (isEmpty()) {
            return false;
        }
        front = (front + 1) % this->size;
        this->count--;
        return true;
    }
    
    int Front() {
        return isEmpty() ? -1 : this->arr[(this->front + 1) % this->size];
    }
    
    int Rear() {
        return isEmpty() ? -1 : this->arr[(this->rear + size - 1) % this->size];
    }
    
    bool isEmpty() {
        return this->count == 0;
    }
    
    bool isFull() {
        return this->count == this->size;
    }
private:
    int* arr;
    int front, rear, size, count;
};

/**
 * Your MyCircularQueue object will be instantiated and called as such:
 * MyCircularQueue* obj = new MyCircularQueue(k);
 * bool param_1 = obj->enQueue(value);
 * bool param_2 = obj->deQueue();
 * int param_3 = obj->Front();
 * int param_4 = obj->Rear();
 * bool param_5 = obj->isEmpty();
 * bool param_6 = obj->isFull();
 */
```

------

## Summary

&emsp;&emsp;这是一道比较复杂的设计题，在涉及到队列的操作时，主要是把握好头尾指针的变化。同时在处理循环数组时，很重要的思路就是模运算。这道题目的分享到这里，感谢你的支持！
