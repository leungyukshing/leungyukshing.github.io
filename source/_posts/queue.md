---
title: Queue
tags:
  - STL
  - Queue
abbrlink: 41872
date: 2018-01-31 23:47:07
---
## Introduction

This post will give a general introduction of Queue, and some of its basic operations.

### Definition
Similar to its meaning in ordinary English, a queue is defined as a waiting line, like a line of people waiting to purchase tickets, where the first person in line is the first person served.For computer applications, we similarly define a **queue** to be a list in which all additions to the list are made at one end, and all deletions from the list are made at the other end. Queues are also called first-in first-out lists, or FIFO.

The entry in a queue ready to be **served**, that is, the first entry that will be removed from the queue, is called the **front** of the queue. Similarly, the last entry in the queue, that is, the one most recently **append**, is called the **rear** of the queue.
<!-- more -->

### Implementation
Now we use C++ to implement a queue. Because I use a static array to store the entries of a queue, so it would be more efficient to make it circular in order to make full use of each storage space.

First I will show you the definition of the stack.(CircularQueue.hpp)
```C++
#define MAXQUEUE 10

template <class Queue_entry>
class CircularQueue {
public:
	CircularQueue();
	bool empty() const;
	Error_code serve();
	Error_code append(const Queue_entry &item);
	Error_code retrieve(Queue_entry &item) const;
protected:
	int count;
	int front, rear;
	Queue_entry entry[MAXQUEUE];
};
```
Here we use count to show how many entries are in the queue. And we use two int variables *front*, *rear* to mark the first entry and last entry in the queue.

#### CircularQueue()
*precondition*: None.
*postcondition*: The Queue has been created and is initialized to be empty.
```C++
template <class Queue_entry>
CircularQueue<Queue_entry>::CircularQueue() {
  //The queue is initialized to be empty
  count = 0;
  rear = MAXQUEUE - 1;
  front = 0;
}

```

#### empty()
*precondition*: None.
*postcondition*: Return **true** if the Queue is empty, otherwise return **false**
```C++
template <class Queue_entry>
bool CircularQueue<Queue_entry>::empty() const { return count == 0; }
```

#### serve()
*precondition*: None.
*postcondition*：If the Queue is not empty, the front of the Queue has been removed, and return Success. Otherwise an Error_code of UnderFlow is returned.
```C++
template <class Queue_entry>
Error_code CircularQueue<Queue_entry>::serve() {
  //The front of the queue is removed.
  if (count <= 0) {
    return UnderFlow;
  }

  count--;
  front = ((front + 1) == MAXQUEUE) ? 0 : (front + 1);
  return Success;
}
```

#### append(**const** Queue_entry &item)
*precondition*: None.
*postcondition*: If there is space, x is added to the Queue as its rear, and return Success. Otherwise an Error_code of OvewrFlow is returned.
```C++
template <class Queue_entry>
Error_code CircularQueue<Queue_entry>::append(const Queue_entry &item) {
  //item is added to the rear of the queue
  if (count >= MAXQUEUE) {
    return OverFlow;
  }
  count++;
  rear = ((rear + 1) == MAXQUEUE) ? 0 : (rear + 1);
  entry[rear] = item;
  return Success;
}
```

#### retrieve(Queue_entry &item)
*precondition*: None.
*postcondition*: If the Queue is not empty, the front of the Queue has been recorded as item, and return Success. Otherwise an Error_code of UnderFlow is returned.
```C++
template <class Queue_entry>
Error_code CircularQueue<Queue_entry>::retrieve(Queue_entry &item) const {
  //The front of the queue retrieved to the output patameter item
  if (count <= 0) {
    item = -1;
    return UnderFlow;
  }
  item = entry[front];
  return Success;
}
```

Hereto, I've introduce the Queue structure.A more useful extended queue is introduced in another post, just search **ExtendtedQueue** in this website. Hope this post can be helpful to you. Thank you!
