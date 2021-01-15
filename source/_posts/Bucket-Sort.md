---
title: 桶排序算法分析及实现
tags:
mathjax: true
date: 2021-01-15 23:32:58

---

## Introduction

&emsp;&emsp;桶排序是一种基于**非比较**的排序方式，我们一般认为它的时间复杂度是$O(n)$，因此它被称为**线性排序**。从这个时间复杂度看，它比绝大多数的排序算法都要好，就连我们常用的归并排序、快排也都是$O(nlogn)$的复杂度，究竟桶排序有没有这么神奇呢？今天我们一探究竟。

------

## 主要思路

&emsp;&emsp;桶排序的主要思路是，把需要排序的一组数据分配到几个桶中，这些桶之间是有序的。分配完后，再对每个桶进行快排，这样桶之间、桶内部都是有序的，我们只需要从小到大遍历一遍桶，依次拿出里面的元素，那么得到的结果就是有序的。

------

## 算法复杂度分析

&emsp;&emsp;假设我们要对`n`个元素排序，一共有`m`个桶，那么每个桶中的元素平均为`n/m`。每个桶内快排的复杂度是$O(\frac{n}{m}\log(\frac{n}{m}))$，那么`m`个桶的快排就是$O(n\log(\frac{n}{m}))$。然后这里有一个假设是当桶的个数`m`和元素个数`n`非常接近的时候，最终的时间复杂度就是$O(n)$。因此推出来这个线性的复杂度的限制条件是非常苛刻的：**每个桶之间的数据分布是均匀的，而且尽可能多的桶。**

------

## C++实现

```c++
#include <iostream>
#include <vector>
#include <map>
using namespace std;

const int BUCKET_NUM = 10;
struct Node {
    int val;
    Node* next;
    Node() {
      this->val = 0;
      this->next = NULL;
    }
    Node(int val) {
      this->val = val;
      this->next = NULL;
    }
};

Node* insert_bucket(Node* head, int value) {
    int pos = -1;
    Node* newNode = new Node(value);

    if (!head) {
      	return newNode;
    }
    if (head->val>value) {
        // new head
        newNode->next = head;
        return newNode;
    }

    Node* pre = head, *cur = head->next;
    while (!cur && cur->val < value) {
        pre = cur;
        cur = cur->next;
    }
    newNode->next = cur;
    pre->next = newNode;
    return head;
}

vector<int> get_bucket(Node* head) {
	vector<int> result;
	if (!head) {
			return result;
	}
	while (head) {
      result.push_back(head->val);
      head = head->next;
	}
	return result;
}

vector<int> bucket_sort(vector<int>& array) {
    int size = array.size();
    int min = INT_MAX, max = INT_MIN;
    for (int i = 0; i < size; i++) {
        if (array[i] < min) {
            min = array[i];
        }
        if (array[i] > max) {
            max = array[i];
        }
    }

    int bucket_size = (max - min) / BUCKET_NUM - 1;
    Node* buckets[BUCKET_NUM];
    for (int i = 0; i < BUCKET_NUM; i++) {
        buckets[i] = NULL;
    }
    for (int i = 0; i < size; i++) {
        int key = array[i] / bucket_size; // bucket number
        buckets[key] = insert_bucket(buckets[key], array[i]);
    }	

    vector<int> result;
    for (int i = 0; i < BUCKET_NUM; i++) {
        vector<int> temp = get_bucket(buckets[i]);
        result.insert(result.end(), temp.begin(), temp.end());
    }
    return result;
}

int main() {
		int arr[] = {44, 726, 23, 8, 19, 111};
		vector<int> nums(arr, arr + 6);
		vector<int> result = bucket_sort(nums);
	
		for (int i = 0; i < result.size(); i++) {
			cout << result[i] << " ";
		}
		cout << endl;
		return 0;
}
```

&emsp;&emsp;为了节省空间，这里每个桶我使用了链表来实现，同时在插入的时候就做了顺序保证。

------

## 小结

&emsp;&emsp;通过这篇博客相信大家对桶排序的基本原理和复杂度有了一个初步的了解，桶排序虽然时间复杂度在数学推理中是线性的，但是在日常的使用中非常难达到这个效果。所以在很多场景下我们依然更倾向于使用快排或者归并排序。

------

## Reference

1. [C++桶排序算法的实现与改进](https://blog.csdn.net/misayaaaaa/article/details/66969486)
2. [桶排序的时间复杂度分析](https://blog.csdn.net/shengqianfeng/article/details/100074146)
