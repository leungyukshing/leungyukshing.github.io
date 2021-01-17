---
title: 最小堆的C++实现
tags:
mathjax: true
date: 2021-01-18 00:25:18


---

## Introduction

&emsp;&emsp;堆是一种很重要的数据结构，它是优先队列的底层，而优先队列是软件工程实践中经常会使用到的一个数据结构，所以今天我们就来看看堆是怎么去实现优先队列的。

------

## 原理

&emsp;&emsp;堆实际上是一颗完全二叉树，对于最小堆而言，非叶子节点的值不大于左孩子和右孩子的值，所以维护起来根节点就是最小的。这样如果我们把节点的值看作是权重（1最大，10最小），这样就是一个优先队列了。

 &emsp;&emsp;堆的操作有两个：插入和取出。

- 取出操作就是取出根节点（最小的值），取出后需要调整二叉树的结构。因为我们需要维护根节点是最小的，所以我们就要从这棵二叉树中找出最小的节点。
- 插入操作就是插入一个数字到堆中，然后维护堆的性质。

&emsp;&emsp;接下来我们就来看看这两个操作如何实现。先看**取出**，取走根节点后，我们需要在全局找一个最小的节点去替换，需要从上到下一直寻找，不断交换父节点和子节点的值即可。因为这个过程是自上到下的，所以也称作下沉`shiftDown()`。

&emsp;&emsp;再看**插入**，因为堆是一颗完全二叉树，所以插入的时候直接插在最后一个空节点，然后再不断向上检查与父节点的大小关系是否满足，再做调整。因为这个过程是自下到上的，所以也称作上浮`shiftUp()`。

------

## C++实现

&emsp;&emsp;一般来说对于一颗完全二叉树来说，都会使用数组存储。而父子节点的下标有如下规律：父节点的下标是`i`，则其左子节点的下标是`2 * i + 1`。

```c++
#include <iostream>
using namespace std;


const int default_size = 100;

template <class T>
class MinHeap {
public:
     MinHeap(int size = default_size);
     MinHeap(T arr[], int n);
    ~ MinHeap();

    bool Insert(const T &x);
    bool Remove();
    int GetMin();
    bool IsEmpty() const;
    bool IsFull() const;
private:
    T* arr; // dynamice array
    int currentSize;
    void shiftDown(int start, int m);
    void shiftUp(int start);
};

template <class T>
MinHeap<T>::MinHeap(int size) {
    arr = new T[size];
    currentSize = 0;
}

template <class T>
MinHeap<T>::MinHeap(T arr[], int n) {
    this->arr = new T[default_size];

    currentSize = n;
    for (int i = 0; i < n; i++) {
        cout << arr[i] << endl;
        this->arr[i] = arr[i];
    }
    int currentPos= currentSize / 2 - 1;
    while (currentPos >= 0) {
        shiftDown(currentPos, currentSize - 1);
        currentPos--;
    }
}

template <class T>
MinHeap<T>::~MinHeap() {
    delete []arr;
    arr = NULL;
    currentSize = 0;
}

template <class T>
bool MinHeap<T>::Insert(const T &x) {
    if (currentSize == default_size) {
      	return false;
    }
    arr[currentSize] = x;
    shiftUp(currentSize);
    currentSize++;
    return true;
}

template <class T>
bool MinHeap<T>::Remove() {
    int x = -1;
    if (IsEmpty()) {
      	return -1;
    }

    x = arr[0];
    arr[0] = arr[currentSize - 1];
    currentSize--;
    shiftDown(0, currentSize - 1);
    return x;
}

template <class T>
int MinHeap<T>::GetMin() {
    if (IsEmpty()) {
      	return -1;
    }
    return this->arr[0];
}

template <class T>
bool MinHeap<T>::IsEmpty() const {
		return currentSize == 0;
}

template <class T>
bool MinHeap<T>::IsFull() const {
		return currentSize == default_size;
}

template <class T>
void MinHeap<T>::shiftDown(int start, int m) {
    int i = start, child = 2 * i + 1; // left node

    T temp = arr[i];
    while (child <= m) {
        if (child < m && arr[child] > arr[child + 1]) {
            // choose the larger child
            child++;
        }
        if (temp <= arr[child]) {
          	break;
        } else {
            arr[i] = arr[child];
            i = child;
            child = child * 2 + 1; // left node
        }
    }
    arr[i] = temp;
}

template <class T>
void MinHeap<T>::shiftUp(int start) {
    int i = start, parent = (i - 1) / 2; // parent

    T temp = arr[i];
    while (i > 0) {
        if (arr[parent] <= temp) {
          	break;
        } else {
            arr[i] = arr[parent];
            i = parent;
            parent = (i - 1) / 2; // parent
        }
    }
    arr[i] = temp;
}


int main() {
    cout << "MinHeap Test" << endl;
    int arr[] = {9, 17, 65, 23, 45, 78, 87, 53};
    int size = sizeof(arr) / sizeof(arr[0]);
    MinHeap<int> heap = MinHeap<int>(arr, size);
    cout << "Initialize MinHeap Done" << endl;
    cout << "GetMin() " << heap.GetMin() << endl;
    heap.Insert(3);
    cout << "GetMin() " << heap.GetMin() << endl;
    cout << "IsEmpty() " << heap.IsEmpty() << endl;
    cout << "IsFull() " << heap.IsFull() << endl;

    while (!heap.IsEmpty()) {
        heap.Remove();
        cout << "GetMin() " << heap.GetMin() << endl;
    }
    cout << "IsEmpty() " << heap.IsEmpty() << endl;
    return 0;
}
```

------

## 小结

&emsp;&emsp;通过这篇博客，相信大家已经了解到优先队列的底层——堆。也同时了解了一个简单的最小堆是如何实现的，其实最大堆的原理也一样。后续如果有和堆相关的一些技巧和知识也会继续和大家分享，感谢你的支持！

------

## Reference

1. [最小堆构建、插入、删除的过程图解](
