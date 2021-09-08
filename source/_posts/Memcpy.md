---
title: C++之memcpy你真的懂了吗
mathjax: true
abbrlink: 52801
date: 2021-09-07 13:55:06
tags:
---

# Introduction

&emsp;&emsp;相信大家开始学习C/C++的时候，都会遇上`memcpy`这个函数。简单来说这个是一个拷贝函数，使用方法很简单，但在今天这篇博客，我将来探讨一下里面的实现细节，尝试自己也实现一个具备基本功能的`memcpy`。

<!-- more -->

# Analysis

&emsp;&emsp;我们先来看一下基本的拷贝思路，我们知道拷贝都是按照字节的，所以我们使用`char*`来控制每次拷贝的粒度都是`1 byte`。于是有个问题，为什么每次只能拷贝1个字节，能不能使用`int *`每次拷贝4个字节，这样理论上是会更加快，其实是可以的。但是因为实现起来代码量增加了，而且效率提升不是特别明显，所以没有必要这样做。

&emsp;然后我们来看另一个重点问题：内存重叠。假设`src`的地址是0，`dst`的地址是10，拷贝的数据长度是50个字节，当我们从`src`把第一个字节的数据拷贝到`dst`的时候，发现`src`的数据脏了，这是因为`dst`本身就在需要拷贝的数据范围中。那么我们再来看另一种情况，假设`dst`的地址是0，`src`的地址是10，拷贝的数据长度仍然是50字节，这样就不会存在上面的问题。再看最后一种情况，当`src`的地址是0，`dst`的地址是10，拷贝的数据长度是8个字节，这样也不会有重叠的影响，所以我们就要重点处理第一种情况。

&emsp;&emsp;在内存拷贝中尽量遵守**从高位向低位拷贝**的原则，这样能最大限度地避免由于内存重叠带来的脏数据问题，因此在解决第一个问题时，我们需要从后向前（高位到低位）拷贝字节。

# Code

```c++
#include <iostream>
using namespace std;

void memcpyV1(void *dst, const void *src, size_t count) {
  char* d;
  const char *s;

  if ((dst < src) || ((char *)dst > ((char *)src + count))) {
    cout << "option1" << endl;
    d = (char *)dst;
    s = (char *)src;
    while (count--) {
      *d++ = *s++;
    }
  } else {
    cout << "option2" << endl;
    d = ((char *)dst + count - 1);
    s = ((char *)src + count - 1);
    *d-- = *s--;
  }
}

int main() {
  int dst[5] = {0};
  int src[] = {1, 2, 3, 4, 5};
  cout << "sizeof(src) = " << 5 * sizeof(int) << endl;
  cout << "begin memcpy" << endl;
  memcpyV1(dst, src, sizeof(src));
  cout << "end memcpy" << endl;
  for (int i = 0; i < 5; i++) {
    cout << dst[i] << " ";
  }
  cout << endl;
  return 0;
}
```

# Summary

&emsp;&emsp;`memcpy`是一个非常基础的函数，但是里面的实现涉及到了程序设计很基本的内存概念。在实现中要特别注意内存的位置，以及指针的用法。希望这篇博客能够有帮助，谢谢你的支持！