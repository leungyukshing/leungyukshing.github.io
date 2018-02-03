---
title: ExtendedQueue
tags:
  - STL
  - Queue
abbrlink: 50481
date: 2018-02-01 15:09:23
---

## Introduction
In the previous post, I've talked about Queue. In this post, I'd like to extend it to make it more useful by adding several new methods.
<!-- more -->

### Implementation

Based on the previous definition of Queue, we use one of C++'s characteristics——Inheritance to define ExtendedQueue(ExtendedQueue.hpp).
```C++
class ExtendedQueue: public Queue {
public:
  bool full() const;
  int size() const;
  void clear();
  Error_code serve_and_retrieve(Queue_entry &item);
};
```
Noted that **serve_and_retrieve()** is actually a combination of **serve()** and **retrieve()**.

#### full()
*precondition*: None.
*postcondition*: Return **true** if the ExtendedQueue is full; return **false** otherwise.
```C++
template <class Queue_entry>
bool ExtendedQueue<Queue_entry>::full() const {
  if (size() == MAXQUEUE) {
    return true;
  }
  return false;
}
```

#### clear()
*precondition*: None.
*postcondition*: All entries in the ExtendedQueue have been removed; it is now empty.
```C++
template <class Queue_entry>
void ExtendedQueue<Queue_entry>::clear() {
  count = 0;
  rear = MAXQUEUE - 1;
  front = 0;
}
```

#### size()
*precondition*: None.
*postcondition*: Return the number of entries in the ExtendedQueue.
```C++
template <class Queue_entry>
int ExtendedQueue<Queue_entry>::size() const { return count; }
```

#### serve_and_retrieve(Queue_entry &item)
*precondition*: None.
*postcondition*: Return UnderFlow if the ExtendedQueue is empty. Otherwise remove and copy the item at the front of the ExtendedQueue to item and return Success.
```C++
template <class Queue_entry>
Error_code ExtendedQueue<Queue_entry>::serve_and_retrieve(Queue_entry &item) {
  //The front of the queue is removed.
  if (count <= 0) {
    return UnderFlow;
  }

  count--;
  item = entry[front];
  front = ((front + 1) == MAXQUEUE) ? 0 : (front + 1);
  return Success;
}
```

So far, I've gave a brief introduction of an entended queue. Please make sure that your each container written by yourself should be strictly tested before being used by another programme. Thank you!
