---
title: LinkedQueue
tags:
  - STL
  - Queue
abbrlink: 48950
date: 2018-02-03 00:46:32
---

## Introduction
I've introduced the Queue structure in previous post. In contiguous storage, queues were significantly harder to manipulate than were stacks, because it was necessary to treat straight-line storage as though it were arranged in a circle, and the extreme cases of full queues and empty queues caused difficulties. As a result, I introduce you the linked queue in this post.
<!-- more -->

### Implementation

Based on the previous definition of Queue, I'll give the definition of LinkedQueue.(LinkedQueue.hpp)
```C++
template <class Queue_entry>
class LinkedQueue {
public:
  LinkedQueue();
  bool empty() const;
  Error_code append(const Queue_entry &item);
  Error_code serve();
  Error_code retrieve(Queue_entry &item) const;

  // safety feartures for linked structures
  ~LinkedQueue();
  LinkedQueue(const LinkedQueue<Queue_entry> &original);
  void operator = (const LinkedQueue<Queue_entry> &original);
  bool operator == (const LinkedQueue<Queue_entry> &original);
  void clear();

  // Extended methods
  int size() const;
  Error_code serve_and_retrieve(Queue_entry &item);
private:
  Node<Queue_entry> *front, *rear;
};
```
We need only keep two pointers, **front** and **rear**, which will point respectively, to the beginning and the end of the queue.

#### LinkedQueue()
*preposition*: None.
*postposition*: The Queue is initialized to be empty.
```C++
template <class Queue_entry>
LinkedQueue<Queue_entry>::LinkedQueue() {
  front = rear = NULL;
}
```

#### empty()
*preposition*: None.
*postposition*: Return **true** if the Queue is empty, otherwise return **false**
```C++
template <class Queue_entry>
bool LinkedQueue<Queue_entry>::empty() const {
  return this->front == NULL;
}
```

#### append(**const** Queue_entry &item)
*preposition*: None.
*postposition*: Add item to the rear of the Queue and return a code of Success or return a code of OverFlow if dynamic memory is exhausted.
```C++
template <class Queue_entry>
Error_code LinkedQueue<Queue_entry>::append(const Queue_entry &item) {
  Node<Queue_entry>* new_rear = new Node<Queue_entry>(item);
  //System's memory is exhausted
  if (new_rear == NULL) {
    return OverFlow;
  }
  if (rear == NULL) {
    front = rear = new_rear;
  }
  else {
    rear->next = new_rear;
    rear = new_rear;
  }
  return Success;
}
```

#### serve()
*preposition*: None.
*postposition*: The front of the Queue is removed. If the Queue is empty, return an Error_code of UnderFlow.
```C++
template <class Queue_entry>
Error_code LinkedQueue<Queue_entry>::serve() {
  if (front == NULL) {
    return UnderFlow;
  }
  Node<Queue_entry> *old_front = front;
  front = old_front->next;
  if (front == NULL) {
    rear = NULL;
  }
  delete old_front;
  return Success;
}
```

#### retrieve(Queue_entry &item)
*preposition*: None.
*postposition*:If the Queue is not empty, the front of the Queue has been recorded as item, and return Success. Otherwise an Error_code of UnderFlow is returned.
```C++
template <class Queue_entry>
Error_code LinkedQueue<Queue_entry>::retrieve(Queue_entry &item) const {
  if (front != NULL) {
    item = front->entry;
    return Success;
  }
  else {
    return UnderFlow;
  }
}
```

#### ~LinkedQueue()
*preposition*: None.
*postposition*: Release the queue itself.
```C++
template <class Queue_entry>
LinkedQueue<Queue_entry>::~LinkedQueue() {
  clear();
}
```

#### LinkedQueue(**const** LinkedQueue<Queue_entry> &original)
*preposition*: None.
*postposition*: The Queue is initialized as a copy of Queue original.
```C++
template <class Queue_entry>
LinkedQueue<Queue_entry>::LinkedQueue(const LinkedQueue<Queue_entry> &original) {
  if (original.empty()) {
    this->front = NULL;
    this->rear = NULL;
  }
  else if (original.front == original.rear) {
    this->front = this->rear = new Node<Queue_entry>(original.front->entry);
  }
  else {
    Node<Queue_entry>* p1 = original.front;
    this->front = new Node<Queue_entry>(p1->entry);
    Node<Queue_entry>* p2 = this->front;
    while (p1 != original.rear) {
      p1 = p1->next;
      p2->next = new Node<Queue_entry>(p1->entry);
      p2 = p2->next;
    }
    this->rear = p2;
  }
}
```

#### operator=(**const** LinkedQueue< Queue_entry > &original)
*preposition*: Node.
*postposition*: The Queue is reset as a copy of Queue original.
```C++
template <class Queue_entry>
void LinkedQueue<Queue_entry>::operator=(const LinkedQueue<Queue_entry> &original) {
  if (*this == original) {
    return;
  }
  clear();
  if (original.empty()) {
    this->front = NULL;
    this->rear = NULL;
  }
  else if (original.front == original.rear) {
    this->front = this->rear = new Node<Queue_entry>(original.front->entry);
  }
  else {
    Node<Queue_entry>* p1 = original.front;
    this->front = new Node<Queue_entry>(p1->entry);
    Node<Queue_entry>* p2 = this->front;
    while (p1 != original.rear) {
      p1 = p1->next;
      p2->next = new Node<Queue_entry>(p1->entry);
      p2 = p2->next;
    }
    this->rear = p2;
  }
}
```

#### operator == (**const** LinkedQueue< Queue_entry > &original)
*preposition*: None.
*postposition*: Return **true** if the Queue is the same as the Queue original, otherwise return **false**.
```C++
template <class Queue_entry>
bool LinkedQueue<Queue_entry>::operator == (const LinkedQueue<Queue_entry> &original) {
  Node<Queue_entry>* p1 = this->front;
  Node<Queue_entry>* p2 = original.front;

  while (p1 != NULL) {
    if (p2 != NULL && p1->entry == p2->entry) {
      p1 = p1->next;
      p2 = p2->next;
      continue;
    }
    else {
      return false;
    }
  }
  if (p2 != NULL) {
    return false;
  }
  return true;
}
```

#### clear()
*preposition*: None.
*postposition*: Clear all the items in the queue.
```C++
template <class Queue_entry>
void LinkedQueue<Queue_entry>::clear() {
  Node<Queue_entry>* p1 = this->front;
  while (p1 != NULL) {
    Node<Queue_entry>* p2 = p1;
    p1 = p1->next;
    delete p2;
  }
  this->front = this->rear = NULL;
}
```

#### size()
*preposition*: None.
*postposition*: Return the number of entries in the Queue.
```C++
template <class Queue_entry>
int LinkedQueue<Queue_entry>::size() const {
  int size = 0;
  Node<Queue_entry> *p1 = this->front;
  while (p1 != NULL) {
    p1 = p1->next;
    size++;
  }
  return size;
}
```

#### serve_and_retrieve(Queue_entry &item)
*preposition*: None.
*postposition*: Return UnderFlow if the Queue is empty. Otherwise remove and copy the item at the front of the Queue to item and return Success.
```C++
template <class Queue_entry>
Error_code LinkedQueue<Queue_entry>::serve_and_retrieve(Queue_entry &item) {
  if (front == NULL) {
    return UnderFlow;
  }
  Node<Queue_entry> *old_front = front;
  front = old_front->next;
  if (front == NULL) {
    rear = NULL;
  }
  item = old_front->entry;
  delete old_front;
  return Success;
}
```

Hereto, I've show you the Queue by using linked storage. This dynamic way to store elements enables us to handle data more conveviently and handy. Hope this post will be helpful to you. Thank you so much!
