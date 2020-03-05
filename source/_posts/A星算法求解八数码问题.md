---
title: A*算法求解八数码问题
tags:
  - AI Algorithm
mathjax: true
abbrlink: 51399
date: 2018-12-26 00:10:38
---
### 问题介绍

&emsp;&emsp;八数码问题也称为九宫问题。在3x3的棋盘，摆有八个棋子，每个棋子上标有1至8的某一数字，不同棋子上标的数字不相同。棋盘上还有一个空格，与空格相邻的棋子可以移到空格中。要求解决的问题是：给出一个初始状态和一个目标状态，找出一种从初始状态转变成目标状态的移动棋子步数最少的移动步骤。
<!-- more -->
------

### 算法介绍

&emsp;&emsp;A星算法，是一种在图形平面上，有多个节点的路径，求出最低通过成本的算法。该算法综合了最良优先搜索和Dijkstra算法的优点：在进行启发式搜索提高算法效率的同时，可以保证找到一条最优路径（基于评估函数）。

&emsp;&emsp;算法的核心在于估值函数。以$g(n)$表示从起点到任意顶点$n$的实际距离，$h(n)$表示任意顶点$n$到目标顶点的估算距离（根据所采用的评估函数的不同而变化），那么A*算法的估值函数为：
$$
f(n) = g(n) + h(n)
$$
&emsp;&emsp;这个公式有如下特点：

- 如果$g(n)=0$，即只计算任意顶点n到目标的评估函数$h(n)$，而不计算起点到顶点n的距离，则算法转化为使用贪心策略的最良优先搜索，速度最快，但不一定能够得到最优解。
- 如果$h(n)$不大于顶点n到目标顶点的实际距离，则一定可以求出最优解，而且$h(n)$越小，需要计算的节点越多，算法效率越低，常见的评估函数有——欧几里得距离、曼哈顿距离等。
- 如果$h(n)=0$，即只需求出起点到任意顶点$n$的最短路径$g(n)$，而不计算任何评估函数$h(n)$，则转化为Dijkstra算法，此时需要计算最多的顶点。

------

### 算法实现

#### 八数码

&emsp;&emsp;本实验是用A*算法求解八数码问题，首先确定一些前提条件。我们使用矩阵的形式存储数据，然后定义一个struct，名为state，用于记录每一个状态的信息，包括棋盘状态、搜索深度、parent等属性，其中parent属性是用于构成最佳路径的链表。

&emsp;&emsp;然后实现估值函数，这是A*算法的关键。这里我们使用三种估值函数：

- 曼哈顿距离：x、y下标与目标对应x、y之差的和
- 欧几里得距离：与目标状态的直线距离
- 错误放置的个数

&emsp;&emsp;接着我们就可以实现A\*算法了。算法的步骤如下：我们维护两个表，open表和close表，每次从open表中选出估值最低的节点进行扩展。open表可以理解为一个存放**未搜索节点**的表。close表可以理解为一个存放**已完成搜索的节点**的表。但是这两个表中的元素在一定情况下可以相互转化。算法规则如下：

- 规则1：对于新添加的节点S（open表和close表中均没有这个状态），将S添加到open表中
- 规则2：对于已经添加的节点S（S在open表中或在close表中）。若S在open表中，与原来的状态$S_0$的$f(n)$比较，取较小的一个。若S在close表中，分两种情况讨论。第一，close表中的原来状态$S_0$的$f(n)$小于S，不做修改；第二，$S_0$的$f(n)$大于S，需要将close表中的$S_0$的$f(n)$更新，然后将该状态移入到open表中。
- 规则3：下一个搜索节点，选择open表中$f(n)$最小的状态作为下一个要扩展的节点。
- 规则4：每一次要生成将待扩展的节点的所有子状态，并按照规则1或规则2更新open表和close表。扩展完该节点后，将该节点放入close表中。

&emsp;&emsp;然后是**是否可求解问题**。我们要注意到一点，不是随机生成一个八数码就可以求解的，我们需要判断其是否有解，这里用到了逆序数的方法。将矩阵写成一维形式，以0代替空格位置。这里直接给出一个判断的结论：**一个状态表示为一维的形式，求出除0之外所有数字的逆序数之和，也就是每个数字前面比它大的数字的个数的和，成为这个状态的逆序。若两个状态的逆序奇偶性相同，则可互相到达，否则不可互相到达**。我们给出规定的八数码问题终止状态，其逆序数为7，因此只需要**判断起始状态的逆序数是否为奇数**即可。

#### 九数码

&emsp;&emsp;九数码问题是基于八数码问题拓展而来的，总体的算法基本一样，只需要微调几个地方就可以了。从估值函数的角度看，两个问题是不同的，因为八数码问题在考虑估值的时候是不讨论空格的，而九数码问题则将0看成是一般元素，估值的时候需要考虑在内，因此他们的估值一般是不同的（当然也要看估值的方式）。

------

### 关键代码分析

#### 八数码

估值函数H（曼哈顿距离）：

```C++
// H: Manhattan Distance
int computeH(state &p) {
  int h = 0;
  for (int i = 0; i < GRID; i++) {
    for (int j = 0; j < GRID; j++) {
      if (p.panel[i][j] != 0) {
        h += abs(rightPos[p.panel[i][j]] / GRID - i);
        h += abs(rightPos[p.panel[i][j]] % GRID - j);
      }
    }
  }
  return h;
}
```

估值函数H1（欧几里得距离）：

```c++
// H1: Euclidean Distance
int computeH1(state &p) {
  int h = 0;
  for (int i = 0; i < GRID; i++) {
    for (int j = 0; j < GRID; j++) {
      if (p.panel[i][j] != 0) {
        int distance = (rightPos[p.panel[i][j]] / GRID - i) * (rightPos[p.panel[i][j]] / GRID - i)
                        + (rightPos[p.panel[i][j]] % GRID - j) * (rightPos[p.panel[i][j]] % GRID - j);
        distance = pow(distance, 0.5);
        h += distance;
      }
    }
  }
  return h;
}
```

估值函数H2（错误放置的个数）：

```c++
// H2: number of wrong position
int computeH2(state &p) {
  int h = 0;
  for (int i = 0; i < GRID; i++) {
    for (int j = 0; j < GRID; j++) {
      if (p.panel[i][j] != 0) {
        if ((3 * i + j) != rightPos[p.panel[i][j]]) {
          h++;
        }
      }
    }
  }
  return h;
}
```

使用逆序数判断状态间是否可到达：

```C++
bool isCanSolve(state &start) {
  int temp[9]= {0};
  int k = 0;
  for (int i = 0; i < GRID; i++) {
    for (int j = 0; j < GRID; j++) {
      temp[k++] = start.panel[i][j];
    }
  }
  int inverseNum = 0;
  for (int i = 0; i < 9; i++) {
    for (int j = i + 1; j < 9; j++) {
      if (temp[i] != 0 && temp[j] != 0 && temp[i] > temp[j]) {
        inverseNum++;
      }
    }
  }
  return (inverseNum % 2 != 0);
}
```

A*算法：

```C++
// apply AStar Algorithm
state* AStar(state &start) {
  int level = 0;
  openTable.push_back(&start);
  int count = 0;

  while (!openTable.empty()) {
    cout << "OpenTable Size: " << openTable.size() << endl;

    // Find the highest f value
    sort(openTable.begin(), openTable.end(), compareState);

    state *p = openTable.back();

    openTable.pop_back();

    // Reach target state
    if (computeH2(*p) == 0) {
      return p;
    }

    level = p->level + 1;

    int zeroPos = findZero(*p);
    int zeroX = zeroPos / 3;
    int zeroY = zeroPos % 3;
    for (int i = 0; i < 4; i++) {
      int x_offset = 0, y_offset = 0;
      switch (i) {
        case 0:
          // right
          x_offset = 0;
          y_offset = 1;
          break;
        case 1:
          // left
          x_offset = 0;
          y_offset = -1;
          break;
        case 2:
          // down
          x_offset = 1;
          y_offset = 0;
          break;
        case 3:
          // up
          x_offset = -1;
          y_offset = 0;
          break;
        default:
          break;
      }

      // out of bound
      if (zeroX + x_offset < 0 || zeroX + x_offset >= GRID || zeroY + y_offset < 0 || zeroY + y_offset >= GRID) {
        continue;
      }
      state *q = new state(level); // Initial a new state
      q->parent = p;
      *q = *p;
      // move zero to the new position
      q->panel[zeroX][zeroY] = q->panel[zeroX + x_offset][zeroY + y_offset];
      q->panel[zeroX + x_offset][zeroY + y_offset] = 0;

      bool isSkip = false;
      vector<state*>::iterator duplicate = findDuplicate(openTable, q);
      // If q is in OpenTable, update it
      if (duplicate != openTable.end()) {
        if (computeF(q) < computeF(*duplicate)) {
          (*duplicate)->level = q->level;
          (*duplicate)->parent = q->parent;
        }
        isSkip = true;
      }
      // If q is in CloseTable, update it
      duplicate = findDuplicate(closeTable, q);
      if (duplicate != closeTable.end()) {
        if (computeF(q) < computeF(*duplicate)) {
          delete *duplicate;
          closeTable.erase(duplicate);
          //cout << "Test: " << q->panel[1][1] << endl;
          openTable.push_back(q);
          isSkip = true;
        }
      }

      if (!isSkip) {
        openTable.push_back(q);
      }
    }

    closeTable.push_back(p);
  }
}
```

#### 九数码

在八数码的基础上做了修改：

估值函数把0也计算，这里以H2为例：

```c++
// H2: number of wrong position
int computeH2(state &p) {
  int h = 0;
  for (int i = 0; i < GRID; i++) {
    for (int j = 0; j < GRID; j++) {
        if ((3 * i + j) != rightPos[p.panel[i][j]]) {
          h++;
        }
    }
  }
  return h;
}
```

A*中生成子状态：

```c++
// Expand sub nodes
    for (int e = 0; e < 9; e++) {
      int x = e / 3;
      int y = e % 3;

      for (int i = 0; i < 4; i++) {
        int x_offset = 0, y_offset = 0;
        switch (i) {
          case 0:
            // right
            x_offset = 0;
            y_offset = 1;
            break;
          case 1:
            // left
            x_offset = 0;
            y_offset = -1;
            break;
          case 2:
            // down
            x_offset = 1;
            y_offset = 0;
            break;
          case 3:
            // up
            x_offset = -1;
            y_offset = 0;
            break;
          default:
            break;
        }

        // out of bound
        if (x + x_offset < 0 || x + x_offset >= GRID || y + y_offset < 0 || y + y_offset >= GRID) {
          continue;
        }
        state *q = new state(level); // Initial a new state
        q->parent = p;
        *q = *p;
        // switch two position
        int temp = q->panel[x][y];
        q->panel[x][y] = q->panel[x + x_offset][y + y_offset];
        q->panel[x + x_offset][y + y_offset] = temp;

        bool isSkip = false;
        vector<state*>::iterator duplicate = findDuplicate(openTable, q);
        // If q is in OpenTable, update it
        if (duplicate != openTable.end()) {
          if (computeF(q) < computeF(*duplicate)) {
            (*duplicate)->level = q->level;
            (*duplicate)->parent = q->parent;
          }
          isSkip = true;
        }

        duplicate = findDuplicate(closeTable, q);
        if (duplicate != closeTable.end()) {
          if (computeF(q) < computeF(*duplicate)) {
            delete *duplicate;
            closeTable.erase(duplicate);
            //cout << "Test: " << q->panel[1][1] << endl;
            openTable.push_back(q);
            isSkip = true;
          }
        }

        if (!isSkip) {
          openTable.push_back(q);
        }
      }
    }
```

------

### 测试与分析

程序运行的结果包括每一次从open表中选出估值最小的节点时open表的大小，以及从初始状态到目标状态的最优路径（每个状态对应的h、g、f值）。为了更好地比较八数码和九数码问题，我们对于同一个数据进行测试，数据共有五组。篇幅有限，这里只展示两组数据，且报告中只展示最优路径，其余信息可在`result`文件夹中找到。

#### Data1（对比不同估值函数$h$和$h_2$）：

输入：

```
0 1 7
6 5 2
3 8 4
```

输出：

使用$h$（曼哈顿距离）：

```
OpenTable Size: 188

Total Run Time: 36 ms.
Step: 0
0 1 7
6 5 2
3 8 4
h: 18	g: 0	f: 18

Step: 1
6 1 7
0 5 2
3 8 4
h: 19	g: 1	f: 20

Step: 2
6 1 7
3 5 2
0 8 4
h: 18	g: 2	f: 20

Step: 3
6 1 7
3 5 2
8 0 4
h: 17	g: 3	f: 20

Step: 4
6 1 7
3 0 2
8 5 4
h: 16	g: 4	f: 20

Step: 5
6 1 7
0 3 2
8 5 4
h: 15	g: 5	f: 20

Step: 6
0 1 7
6 3 2
8 5 4
h: 14	g: 6	f: 20

Step: 7
1 0 7
6 3 2
8 5 4
h: 13	g: 7	f: 20

Step: 8
1 3 7
6 0 2
8 5 4
h: 12	g: 8	f: 20

Step: 9
1 3 7
6 2 0
8 5 4
h: 11	g: 9	f: 20

Step: 10
1 3 0
6 2 7
8 5 4
h: 10	g: 10	f: 20

Step: 11
1 0 3
6 2 7
8 5 4
h: 9	g: 11	f: 20

Step: 12
1 2 3
6 0 7
8 5 4
h: 8	g: 12	f: 20

Step: 13
1 2 3
6 7 0
8 5 4
h: 7	g: 13	f: 20

Step: 14
1 2 3
6 7 4
8 5 0
h: 6	g: 14	f: 20

Step: 15
1 2 3
6 7 4
8 0 5
h: 5	g: 15	f: 20

Step: 16
1 2 3
6 0 4
8 7 5
h: 4	g: 16	f: 20

Step: 17
1 2 3
0 6 4
8 7 5
h: 3	g: 17	f: 20

Step: 18
1 2 3
8 6 4
0 7 5
h: 2	g: 18	f: 20

Step: 19
1 2 3
8 6 4
7 0 5
h: 1	g: 19	f: 20

Step: 20
1 2 3
8 0 4
7 6 5
h: 0	g: 20	f: 20

```

使用$h_2$（错误放置个数）：

```
OpenTable Size: 3198

Total Run Time: 13700 ms.
Step: 0
0 1 7
6 5 2
3 8 4
h: 8	g: 0	f: 8

Step: 1
6 1 7
0 5 2
3 8 4
h: 8	g: 1	f: 9

Step: 2
6 1 7
3 5 2
0 8 4
h: 8	g: 2	f: 10

Step: 3
6 1 7
3 5 2
8 0 4
h: 8	g: 3	f: 11

Step: 4
6 1 7
3 0 2
8 5 4
h: 8	g: 4	f: 12

Step: 5
6 1 7
0 3 2
8 5 4
h: 8	g: 5	f: 13

Step: 6
0 1 7
6 3 2
8 5 4
h: 8	g: 6	f: 14

Step: 7
1 0 7
6 3 2
8 5 4
h: 7	g: 7	f: 14

Step: 8
1 3 7
6 0 2
8 5 4
h: 7	g: 8	f: 15

Step: 9
1 3 7
6 2 0
8 5 4
h: 7	g: 9	f: 16

Step: 10
1 3 0
6 2 7
8 5 4
h: 7	g: 10	f: 17

Step: 11
1 0 3
6 2 7
8 5 4
h: 6	g: 11	f: 17

Step: 12
1 2 3
6 0 7
8 5 4
h: 5	g: 12	f: 17

Step: 13
1 2 3
6 7 0
8 5 4
h: 5	g: 13	f: 18

Step: 14
1 2 3
6 7 4
8 5 0
h: 4	g: 14	f: 18

Step: 15
1 2 3
6 7 4
8 0 5
h: 3	g: 15	f: 18

Step: 16
1 2 3
6 0 4
8 7 5
h: 3	g: 16	f: 19

Step: 17
1 2 3
0 6 4
8 7 5
h: 3	g: 17	f: 20

Step: 18
1 2 3
8 6 4
0 7 5
h: 2	g: 18	f: 20

Step: 19
1 2 3
8 6 4
7 0 5
h: 1	g: 19	f: 20

Step: 20
1 2 3
8 0 4
7 6 5
h: 0	g: 20	f: 20
```

**分析**：通过同一个输入，不同估值函数的搜索效率差别是很大的。从这个例子可以看出，使用$h$的效率更高，其搜索的点更少。这是因为，相比起$h_2$，$h$更符合移动棋子实际所消耗的代价，因而更适合作为估值函数。

#### Data2（对比八数码和九数码）：

输入：

```
0 1 7
6 5 2
3 8 4
```

输出：

```
// 八数码
OpenTable Size: 188

Total Run Time: 36 ms.
Step: 0
0 1 7
6 5 2
3 8 4
h: 18	g: 0	f: 18

Step: 1
6 1 7
0 5 2
3 8 4
h: 19	g: 1	f: 20

Step: 2
6 1 7
3 5 2
0 8 4
h: 18	g: 2	f: 20

Step: 3
6 1 7
3 5 2
8 0 4
h: 17	g: 3	f: 20

Step: 4
6 1 7
3 0 2
8 5 4
h: 16	g: 4	f: 20

Step: 5
6 1 7
0 3 2
8 5 4
h: 15	g: 5	f: 20

Step: 6
0 1 7
6 3 2
8 5 4
h: 14	g: 6	f: 20

Step: 7
1 0 7
6 3 2
8 5 4
h: 13	g: 7	f: 20

Step: 8
1 3 7
6 0 2
8 5 4
h: 12	g: 8	f: 20

Step: 9
1 3 7
6 2 0
8 5 4
h: 11	g: 9	f: 20

Step: 10
1 3 0
6 2 7
8 5 4
h: 10	g: 10	f: 20

Step: 11
1 0 3
6 2 7
8 5 4
h: 9	g: 11	f: 20

Step: 12
1 2 3
6 0 7
8 5 4
h: 8	g: 12	f: 20

Step: 13
1 2 3
6 7 0
8 5 4
h: 7	g: 13	f: 20

Step: 14
1 2 3
6 7 4
8 5 0
h: 6	g: 14	f: 20

Step: 15
1 2 3
6 7 4
8 0 5
h: 5	g: 15	f: 20

Step: 16
1 2 3
6 0 4
8 7 5
h: 4	g: 16	f: 20

Step: 17
1 2 3
0 6 4
8 7 5
h: 3	g: 17	f: 20

Step: 18
1 2 3
8 6 4
0 7 5
h: 2	g: 18	f: 20

Step: 19
1 2 3
8 6 4
7 0 5
h: 1	g: 19	f: 20

Step: 20
1 2 3
8 0 4
7 6 5
h: 0	g: 20	f: 20
```

```
// 九数码
OpenTable Size: 222

Total Run Time: 55 ms.
Step: 0
0 1 7
6 5 2
3 8 4
h: 20	g: 0	f: 20

Step: 1
6 1 7
0 5 2
3 8 4
h: 20	g: 1	f: 21

Step: 2
6 1 7
3 5 2
0 8 4
h: 20	g: 2	f: 22

Step: 3
6 1 7
3 5 2
8 0 4
h: 18	g: 3	f: 21

Step: 4
6 1 7
3 0 2
8 5 4
h: 16	g: 4	f: 20

Step: 5
6 1 7
0 3 2
8 5 4
h: 16	g: 5	f: 21

Step: 6
0 1 7
6 3 2
8 5 4
h: 16	g: 6	f: 22

Step: 7
1 0 7
6 3 2
8 5 4
h: 14	g: 7	f: 21

Step: 8
1 3 7
6 0 2
8 5 4
h: 12	g: 8	f: 20

Step: 9
1 3 7
6 2 0
8 5 4
h: 12	g: 9	f: 21

Step: 10
1 3 0
6 2 7
8 5 4
h: 12	g: 10	f: 22

Step: 11
1 0 3
6 2 7
8 5 4
h: 10	g: 11	f: 21

Step: 12
1 2 3
6 0 7
8 5 4
h: 8	g: 12	f: 20

Step: 13
1 2 3
6 7 0
8 5 4
h: 8	g: 13	f: 21

Step: 14
1 2 3
6 7 4
8 5 0
h: 8	g: 14	f: 22

Step: 15
1 2 3
6 7 4
8 0 5
h: 6	g: 15	f: 21

Step: 16
1 2 3
6 0 4
8 7 5
h: 4	g: 16	f: 20

Step: 17
1 2 3
0 6 4
8 7 5
h: 4	g: 17	f: 21

Step: 18
1 2 3
8 6 4
0 7 5
h: 4	g: 18	f: 22

Step: 19
1 2 3
8 6 4
7 0 5
h: 2	g: 19	f: 21

Step: 20
1 2 3
8 0 4
7 6 5
h: 0	g: 20	f: 20
```

&emsp;&emsp;可以看出，对于同一个例子，使用相同的估值函数，八数码和九数码的搜索图是不同的，因为他们的估值不同，所以选的点可能会不一样，因此整个搜索的路径也是不一样的。

&emsp;&emsp;证明部分的内容也写在这里（注意这里讨论的例子都是八数码问题）。首先证明由A\*算法挑选出来的点$n$必定满足
$$
f(n) \le f^{\*}(S_0)
$$。
&emsp;&emsp;在结果中我们看到，每个状态都有$f(n)$，最后的Step就是$f^{\*}(S_0)$，可以看出他们都满足这个条件。这是因为，A\*算法挑选出来的点$n$位于最优路径上，而这个点对最优路径的估值总是**比实际更加乐观**，因此这个不等式满足。

&emsp;&emsp;然后证明凡通过A\*算法挑选出来的点$n_i$扩展的一个子节点$n_j$，均满足$h_2(n_i) \le 1 + h_2(n_j)$。在上面的例子也可以看到，每一个状态的$h$都满足上述公式。这是因为，对于估值函数$h_2$来说，每走一步，最乐观的情况就是将一个错误放置的棋子移动到正确的位置，即相邻两个状态之间的费用相差最多为1。

&emsp;&emsp;对比八数码和九数码。他们最本质的区别是，虽然算法大致一样，但是九数码问题中的算法已经不是A*算法了。举个例子，对于如下输入：

```
1 2 3
0 8 4
7 6 5
```

$h(n) = 2$，但是$h^{\*}(n)=1$，不满足$h(n) \le h^{\*}(n)$。

------

### 总结

&emsp;&emsp;这个项目我们实现了使用A\*算法求解八数码问题，可以看出，A\*算法作为一种启发式搜索方法，充分利用了评估函数，挑选评估值最优的点来搜索。我认为，算法的关键是选取合适的估值函数。过于乐观的估值将会降低搜索效率。以本实验为例，使用错误放置的个数作为估值函数显然过于乐观，因为需要走的步数肯定比错误放置的个数要多得多；相反，使用曼哈顿距离则好很多，因为曼哈顿距离考虑了横纵坐标之差，这也恰恰是我们移动棋子时所要走的步数，因此估值函数并不是越乐观越好，估计过于乐观有时会显出很高的复杂性。然后我还思考了一下，可以对估值函数做一个组合优化，因为如果只用一种估值函数可能没办法达到很好的搜索路径，我们可以组合多种估值方法，为他们分配不同的权重，来优化估值函数。

&emsp;从算法的角度看，A\*算法的优点是，相比起DFS和BFS，它不是盲目搜索，而是有提示的启发式搜索。但是它需要大量的存储空间来存放已经搜索过的状态节点，以防止重复搜素。而有一种IDA\*算法（Interative deepening A\*，迭代加深A\*算法），它设置最大深度`depth`，若没有到达目标状态则加深`depth`。迭代过程中剪掉$f(n)\gt depth$的路径。它的优点是节省空间，不需要保存大量的中间状态。如果以后有时间，可以将IDA\*实现后与A\*作比较。
