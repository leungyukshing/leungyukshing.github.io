---
title: 归并排序之C++实现
abbrlink: 6011
date: 2019-06-12 18:14:48
tags:
mathjax: true
---

## 介绍

&emsp;&emsp;之前介绍过[快速排序](<http://leungyukshing.cn/archives/QuickSort.html>)，相比起快排，归并排序是stable的。所谓的stable，是指算法的实现保持了相同的元素之间的顺序（the implementation preserves the input order of equal elements in the sorted order）。它采用了分治的思路。从时间复杂度来看，快排平均复杂度是$O(nlogn)$，最坏情况下是$O(n^2)$；归并排序的时间复杂度总是$O(nlogn)$，这是他的优势所在。

<!-- more -->

---

## 思路

&emsp;&emsp;分治的思想是将大的数组分为左右两边，再对这两个子数组进行求解。我们假设划分的这两个子数组经过排序后已经有序，然后我们再合并两个有序序列得到最终的有序数组。

&emsp;&emsp;简单来说，就是将数据划分到一个元素，两个单元素数组合并的时候就完成了最基本的比较，这样一步一步往上走，每步合并两个有序序列，最后得到排序的结果。

---

## 实现

```c++
#include <iostream>
#include <cstdio>
using namespace std;

void Merge(int *A, int *L, int leftCount, int *R, int rightCount) {
  int i, j, k;
  i = 0;
  j = 0;
  k = 0;

  while (i < leftCount && j < rightCount) {
    if (L[i] < R[j])
      A[k++] = L[i++];
    else
      A[k++] = R[j++];
  }
  while (i < leftCount)
    A[k++] = L[i++];
  while (j < rightCount)
    A[k++] = R[j++];
}

void MergeSort(int *A, int n) {
  int mid, i, *L, *R;
  if (n < 2)
    return;

  mid = n / 2;
  L = new int[mid];
  R = new int[n - mid];

  for (i = 0; i < mid; i++)
    L[i] = A[i];
  for (i = mid; i < n; i++)
    R[i - mid] = A[i];

  MergeSort(L, mid);
  MergeSort(R, n - mid);
  Merge(A, L, mid, R, n - mid);

  delete []R;
  delete []L;
}

int main() {
  int A[] = {6, 2, 3, 1, 9, 10, 15, 13, 12, 17};
  int i, n;
  n = sizeof(A) / sizeof(A[0]);

  MergeSort(A, n);
  for (i = 0; i < n; i++) {
    cout << A[i] << " ";
  }
  cout << endl;
  return 0;
}
```

运行结果：![MergeSort](/images/mergesort.png)

---

## 小结

&emsp;&emsp;归并排序和快速排序是两种比较高效的排序算法，对于掌握分治思想也是很有帮助，希望这篇博客能够帮助大家理解归并排序的思路，谢谢！
