---
title: CCF--窗口
tags:
  - CCF
abbrlink: 48026
date: 2018-03-16 13:35:43
---
## 窗口问题
### 问题描述
&emsp;在某图形操作系统中,有 N 个窗口,每个窗口都是一个两边与坐标轴分别平行的矩形区域。窗口的边界上的点也属于该窗口。窗口之间有层次的区别,在多于一个窗口重叠的区域里,只会显示位于顶层的窗口里的内容。
&emsp;当你点击屏幕上一个点的时候,你就选择了处于被点击位置的最顶层窗口,并且这个窗口就会被移到所有窗口的最顶层,而剩余的窗口的层次顺序不变。如果你点击的位置不属于任何窗口,则系统会忽略你这次点击。
&emsp;现在我们希望你写一个程序模拟点击窗口的过程。
<!-- more -->

### 输入格式
&emsp;输入的第一行有两个正整数,即 N 和 M。(1 ≤ N ≤ 10,1 ≤ M ≤ 10)
&emsp;接下来 N 行按照从最下层到最顶层的顺序给出 N 个窗口的位置。 每行包含四个非负整数 x1, y1, x2, y2,表示该窗口的一对顶点坐标分别为 (x1, y1) 和 (x2, y2)。保证 x1 < x2,y1 < y2。
&emsp;接下来 M 行每行包含两个非负整数 x, y,表示一次鼠标点击的坐标。
&emsp;题目中涉及到的所有点和矩形的顶点的 x, y 坐标分别不超过 2559 和 1439。

### 输出格式
&emsp;输出包括 M 行,每一行表示一次鼠标点击的结果。如果该次鼠标点击选择了一个窗口,则输出这个窗口的编号(窗口按照输入中的顺序从 1 编号到 N);如果没有,则输出"IGNORED"(不含双引号)。

### 样例输入
> 3 4
> 0 0 4 4
> 1 1 5 5
> 2 2 6 6
> 1 1
> 0 0
> 4 4
> 0 5

### 样例输出
> 2
> 1
> 1
> IGNORED

### 样例说明
&emsp;第一次点击的位置同时属于第 1 和第 2 个窗口,但是由于第 2 个窗口在上面,它被选择并且被置于顶层。
&emsp;第二次点击的位置只属于第 1 个窗口,因此该次点击选择了此窗口并将其置于顶层。现在的三个窗口的层次关系与初始状态恰好相反了。
&emsp;第三次点击的位置同时属于三个窗口的范围,但是由于现在第 1 个窗口处于顶层,它被选择。
&emsp;最后点击的 (0, 5) 不属于任何窗口。

## 解决方法
### 分析
&emsp;这里和以往处理类似的顺序问题一样，自定义一个**Window**的结构，并且定义了`operator<`的规则，这为后面的排序做好了准备。
&emsp;我们使用数组**windows**来存储题目给出的若干个窗口信息，每个窗口都具有自己的编号和顺序。
&emsp;对于每一个点击操作，首先在数组中找出包含点击点的窗口，程序中将这个功能用`inWindow()`函数实现。由于存在窗口重叠的可能，因此，需需要对该函数返回的vector中的窗口进行顺序的排列，然后输出顺序最靠前的窗口编号，否则输出"IGNORED"。
### 代码实现
```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Windows {
  int x1;
  int x2;
  int y1;
  int y2;
  int order;
  int number;
  Windows() {
    x1 = x2 = y1 = y2 = 0;
    order = -1;
  }
  bool operator<(const Windows& other) const {
    return (order < other.order);
  }
};

std::vector<Windows> inWindow(Windows windows[], int n, int x, int y) {
  std::vector<Windows> v;
  for (int i = 1; i <=n; i++) {
    if (x >= windows[i].x1 && x <= windows[i].x2
        && y >= windows[i].y1 && y <= windows[i].y2) {
      v.push_back(windows[i]);
    }
  }
  return v;
}

int main() {
  int N, M;
  cin >> N >> M;
  Windows windows[N+1];
  for (int i = 1; i <= N; i++) {
    int x1, y1, x2, y2;
    cin >> x1 >> y1 >> x2 >> y2;
    windows[i].x1 = x1;
    windows[i].y1 = y1;
    windows[i].x2 = x2;
    windows[i].y2 = y2;
    windows[i].number= i;
    windows[i].order = N-i+1;
  }

  while (M--) {
    int click_x, click_y;
    cin >> click_x >> click_y;
    std::vector<Windows> tmp = inWindow(windows, N, click_x, click_y);
    if (tmp.size() == 0) {
      cout << "IGNORED" << endl;
    }
    else {
      sort(tmp.begin(), tmp.end());
      /*测试tmp*/
      /*
      for(int k = 0; k < tmp.size(); k++) {
        cout << tmp[k].number << " " << tmp[k].order << endl;
      }
      */
      cout << tmp[0].number << endl;
      for (int i = 1; i<= N; i++) {
        if (windows[i].number == tmp[0].number) {
          windows[i].order = 1;
        }
        else {
          windows[i].order++;
        }
      }
    }
  }
  return 0;
}
```
