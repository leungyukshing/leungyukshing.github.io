---
title: 快速排序之C++实现
abbrlink: 59785
date: 2019-06-07 03:01:20
tags:
mathjax: true
---
## 介绍

&emsp;&emsp;说到排序算法，许多小白脱口而出的都是冒泡排序、插入排序等。但是如果考虑到时间复杂度，上面这几种算法效率就不是这么高了，对于大量的数据排序，我们需要更加快速的排序方法。在这篇博客我们就来看看**快速排序**。

<!-- more -->

---

## 思路

&emsp;&emsp;快速排序的思想也是分治，将一份的数据分成两份，分别排列成有序数组后再合并，这个和**归并排序**是很类似的，但是排序的方法有点不同。

&emsp;&emsp;在快速排序中，每一个数组都有一个**pivot**元素，它是作为这个数组的标杆，我们要将数组分为两部分，比这个**pivot**大的放在它的右边，比它小的放在它的左边，然后再分别对两个数组进行同样的操作。

&emsp;&emsp;从复杂度的角度看，在最优情况下，复杂度为$O(nlogn)$；在最糟糕的情况下，复杂度为**$O(n^2)$**。详细的证明请大家参考<https://www.cnblogs.com/fengty90/p/3768827.html>

## 实现

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
using namespace std;

int partition(vector<int>& entry, int low, int high) {
  int pivot = entry[low];
  int last_small = low;
  for (int i = low + 1; i <= high; i++) {
    if (entry[i] < pivot) {
      last_small++;
      swap(entry[i], entry[last_small]);
    }
  }
  swap(entry[low], entry[last_small]);
  return last_small;
}

void recursive_quickSort(vector<int>& entry, int low, int high) {
  if (low < high) {
    int pivot = partition(entry, low, high);
    recursive_quickSort(entry, low, pivot - 1);
    recursive_quickSort(entry, pivot + 1, high);
  }
}

void quickSort(vector<int>& entry) {
  recursive_quickSort(entry, 0, entry.size() - 1);
}

int main() {
  int a[] = {3, 5, 7, 9, 2, 3, 1, 0, 7, 5, 4};
  vector<int> va(a, a+11);
  cout << "Before QuickSort: " << endl;
  for (int i = 0; i < va.size(); i++) {
    cout << va[i] << " ";
  }
  cout << endl;

  quickSort(va);

  cout << "After QuickSort: " << endl;
  for (int i = 0; i < va.size(); i++) {
    cout << va[i] << " ";
  }
  cout << endl;
  return 0;
}
```

运行结果：![QuickSort](/images/quicksort.png)

---

## 小结

&emsp;&emsp;快速排序几乎可以说是程序员必须掌握的一种排序方法，STL的sort实现也是基于快速排序的，所以希望这篇博客能够帮助到你，谢谢！
