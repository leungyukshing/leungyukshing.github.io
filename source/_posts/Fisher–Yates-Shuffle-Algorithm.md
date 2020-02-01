---
title: Fisher-Yates洗牌算法
date: 2020-02-01 12:05:40
mathjax: true
---

# 洗牌算法

&emsp;&emsp;洗牌算法是非常经典的算法，目的是随机打乱一个有序的数组，怎么做才能高效呢？这个看似简单的问题背后隐藏了着一个非常出名的算法，在这篇博客我就来和大家分享Ronald Fisher和 Frank Yates提出的洗牌算法。

<!-- more -->

## 洗牌问题

&emsp;&emsp;洗牌问题实质上是一个将有序数组打乱成无序数组的问题，可以简单描述为：有一个大小为100的有序数组，里面的元素从1-100按顺序排列，如何把这个数组随机打乱，得到一个随即乱序的数组呢？

&emsp;&emsp;我们先用一个更加简单的问题引入：如何从这个大小为100的有序数组中随机选择**1个数**？

&emsp;&emsp;最直接的做法就是直接调用`random()`，获取到一个0-99的随机数作为下标，然后到数组中拿到对应的数字。接下来我们更近一步，如果要从这个数组中随机选择**50个数**呢？

&emsp;&emsp;按照上面的思路，我们可以调用`random()`50次，获取到50个随机数，但是为了保证下标不重复，我们每一次都需要判断是否重复。解决的方式可以是弄一个额外的数组，把每一次随机的结果都放到里面，下一次随机的时候就遍历这个数组，看看是否已经存在，如果有的话就继续随机，直至这个数组有50个元素。

&emsp;&emsp;上面这种方法可行，但是我们注意到里面有一个遍历数组的操作，越往后面，重复的概率越大，所以我们实际上遍历数组的次数可能远远大于随机的次数。

&emsp;&emsp;上面就说明白了我们直观的做法有什么问题，接下来就介绍Fisher-Yates洗牌算法。

## Fisher-Yates Shuffle Algorithm

### 算法思路

&emsp;&emsp;算法的思路是先将原有的数组打乱，然后按顺序遍历一遍，得到的就是随机乱序的数组。但是这里有一点非常重要，我们要定义什么是随机：**每一个元素被放置在新数组中的第$i$个位置的概率是都是$\frac{1}{n}$（假设数组大小为$n$）**。

&emsp;&emsp;算法概述如下：

1. 初始化原始数组`arr1`和新数组`arr2`。
2. 从`arr1`（假设还剩$k$个）中随机产生一个$[0,k)$之间的数字$p$。
3. 在`arr1`中把第$p$个数取出
4. 重复步骤2、3直至`arr2`里面有$n$个数。

&emsp;&emsp;我在这里简单地证明一下随机性：一个元素$m$在新数组被放入第$i$个位置的概率是$\frac{1}{n}$。这个条件等价于前$i-1$个位置没有选中$m$，并且第$i$个位置选中$m$：
$$
P = \frac{n - 1}{n} \times \frac{n - 2}{n - 1} \times \cdots \times \frac{n - i + 1}{n - i + 2} \times \frac{1}{n - i + 1} \\
= \frac{1}{n}
$$

### 现代方法

&emsp;&emsp;上述算法避开了我们之前遇到的问题，无论生成多少个随机数，计算复杂度都不会随着次数的增加而不断上升，但是应用到计算机还需要做一定的改进。原因是算法二、三步要求的是维护一个剩余元素的数组，计算机做这种操作会耗费很多空间和时间。

&emsp;&emsp;改进的方法很简单，把选择出来的元素交换到数字的最后就可以了，而数组靠前的部分就是未被选择的元素。因此我们需要维护一个$k$，数组中前$k$个元素是未被选择的，后面$n-k$个元素是已经被选择的，当每次迭代$k$都会减1。

### 代码

&emsp;&emsp;我用C++简单地实现了洗牌算法，需要注意，在C++中使用`rand()`的时候，实际上返回的是伪随机数，所以我们要利用时间作为种子去生成。

```c++
#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
using namespace std;

void shuffle(vector<int> &arr) {
  srand(int(time(0)));
  int size = arr.size();
  for (int i = size - 1; i >= 0; i--) {
    int index = rand() % (i + 1);
    int temp = arr[index];
    arr[index] = arr[i];
    arr[i] = temp;
  }
}

int main() {
  int a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  vector<int> arr(a, a + 10);
  int size = arr.size();

  cout << "Before Shuffle: " << endl;
  for (int i = 0; i < size; i++) {
    cout << arr[i] << " ";
  }
  cout << endl;
  cout << endl;

  shuffle(arr);

  cout << "After Shuffle: " << endl;
  for (int i = 0; i < size; i++) {
    cout << arr[i] << " ";
  }
  cout << endl;
  return 0;
}
```

---

## 小结

&emsp;&emsp;原来看上去很简单的一个随机打乱的问题里面还藏着这么多学问，洗牌算法的分享就到这里。欢迎转发、评论，谢谢您的支持！