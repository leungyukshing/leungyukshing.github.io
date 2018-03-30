---
title: 高斯消元法之讲解与代码实现
abbrlink: 5180
date: 2018-03-15 23:45:59
tags:
mathjax: true
---
## 前言
&emsp;在数值计算课程中学到了高斯消元法的详细算法过程，自己就用C++实现了一下。首先我会讲解算法的推导过程，然后我将会把代码分块讲解，想直接看完整代码的可以直接到最后看。
<!-- more -->

## 高斯消元法讲解
### 算法详解
对于一个这样的线性方程组
$$
\begin{cases}
a_{11}x_1 + a_{12}x_2 + \cdots + a_{1n}x_n = b_1\\
a_{21}x_1 + a_{22}x_2 + \cdots + a_{2n}x_n = b_2\\
\vdots \\
a_{n1}x_1 + a_{n2}x_2 + \cdots + a_{nn}x_n = b_n\\
\end{cases} \tag{1.1}
$$
，或写成矩阵形式
$$
        \begin{bmatrix}
        a_{11} & a_{12} & \cdots & a_{1n} \\
        a_{21} & a_{22} & \cdots & a_{2n} \\
        \vdots & \vdots & & \vdots & \\
        a_{n1} & a_{n2} & \cdots & a_{nn}\\
        \end{bmatrix}
        \begin{bmatrix}
        x_1\\
        x_2\\
        \vdots\\
        x_n \\
        \end{bmatrix} =
        \begin{bmatrix}
        b_1\\
        b_2\\
        \vdots\\
        b_n \\
        \end{bmatrix}
$$
，简记为***Ax = b***.
我们可以使用高斯消元法来进行求解。

(1) 第一步（*k=1*）
设$a_{11}^{(1)} ≠ 0$，首先计算乘数
$$m_{i1} = a_{i1}^{(1)} / a_{11}^{(1)}, i = 2,3,...,n.$$
用$-m_{i1}$乘方程组（1.1）的第1个方程，加到第$i$个（$i=2，3，4，...，n$）方程上，消去方程组（1.1）的从第2个方程到第$n$个方程中的未知数$x_1$，得到与方程组（1.1）等价的线性方程组：
$$
\begin{bmatrix}
        a_{11}^{(1)} & a_{12}^{(1)} & \cdots & a_{1n}^{(1)} \\
        0 & a_{22}^{(2)} & \cdots & a_{2n}^{(2)} \\
        \vdots & \vdots & & \vdots & \\
        0 & a_{n2}^{(2)} & \cdots & a_{nn}^{(2)}\\
        \end{bmatrix}
        \begin{bmatrix}
        x_1\\
        x_2\\
        \vdots\\
        x_n \\
        \end{bmatrix} =
        \begin{bmatrix}
        b_1^{(1)}\\
        b_2^{(2)}\\
        \vdots\\
        b_n^{(2)} \\
        \end{bmatrix}
$$
简记为
$$A^{(2)}x = b^{(2)}，$$
其中，$A^{(2)}，b^{(2)}$的元素计算公式为
$$
\begin{cases}
a_{ij}^{(2)} = a_{ij}^{(1)} - m_{i1}a_{1j}^{(1)},i,j = 2,3,...,n.\\
b_i^{(2)} = b_i ^{(1)} - m_{i1}b_1^{(1)}, i = 2,3,...,n.\\
\end{cases}
$$

(2) 第$k$次消元（$k = 1,2,...,n-1$）
假设第$k-1$步已经完成，我们有如下线性方程组：
$$
\begin{bmatrix}
        a_{11}^{(1)} & a_{12}^{(1)} & \cdots & a_{1k}^{(1)} & \cdots &a_{1n}^{(1)} \\
         & a_{22}^{(2)} & \cdots & a_{2k}^{(2)} & \cdots & a_{2n}^{(2)} \\
         & & \ddots & \vdots & & \vdots & \\
         & & & a_{kk}^{(k)} & \cdots & a_{kn}^{(k)}\\
         & & & a_{nk}^{(k)} & \cdots & a_{nn}^{(k)}
        \end{bmatrix}
        \begin{bmatrix}
        x_1\\
        x_2\\
        \vdots\\
        x_k \\
        \vdots \\
        x_n \\
        \end{bmatrix} =
        \begin{bmatrix}
        b_1^{(1)}\\
        b_2^{(2)}\\
        \vdots\\
        b_k^{(k)}\\
        \vdots \\
        b_n^{(k)} \\
        \end{bmatrix},
$$
简记为$A^{(k)}x = b^{(k)}$.
设$a_{kk}^{(k)} ≠ 0$，计算乘数
$$
m_{ik} = a_{ik}^{(k)} / a_{kk}^{(k)}, i = k+1,k+2,...,n.
$$
用$-m_{ik}$乘方程组第$k$个方程，并加到第$i$个方程（$i = k+1,...,n$），消去从第$k+1$个方程到第$i$个方程中的未知数$x_k$，得到$A^{(k+1)}x = b^{(k+1)}$.
其中$A^{(k+1)}， b^{(k+1)}$元素的计算公式为：
$$
\begin{cases}
a_{ij}^{(k+1)} = a_{ij}^{(k)} - m_{ik}a_{kj}^{(k)},i,j = k+1,...,n.\\
b_i^{(k+1)} = b_i ^{(k)} - m_{ik}b_k^{(k)}, i = k+1,...,n.\\
\end{cases}，
$$
显然$A^{(k+1)}中从第1行道第k行与A^^{(k)}$相同。

（3）经过$n-1$步消元计算后，我们得到$A^{(n)}x = b^{(n)}$：
$$
\begin{bmatrix}
        a_{11}^{(1)} & a_{12}^{(1)} & \cdots & a_{1n}^{(1)} \\
         & a_{22}^{(2)} & \cdots & a_{2n}^{(2)} \\
         & & \ddots & \vdots \\
         & & & a_{nn}^{(n)}
        \end{bmatrix}
        \begin{bmatrix}
        x_1\\
        x_2\\
        \vdots \\
        x_n \\
        \end{bmatrix} =
        \begin{bmatrix}
        b_1^{(1)}\\
        b_2^{(2)}\\
        \vdots \\
        b_n^{(k)} \\
        \end{bmatrix},
$$

若**A**是非奇异矩阵，且$a_{kk}^{(k)} ≠ 0（k=1,2,...,n-1）$，求解矩阵的回代公式为：
$$
\begin{cases}
x_n = b_n / a_{nn}^{(n)},\\
x_k = b_k^{(k)} - \sum_{j=k+1}^n a_{ki}^{(k)}x_j / a_{kk}, k = n-1,n-2,...,1.\\
\end{cases}，
$$
这样，高斯的消元与回代就结束了，我们就可以得到x的解。
### 变量定义
&emsp;高斯消元法主要是矩阵的运算，这里我们采用二维数组的形式存储矩阵，采用一维数组的形式存储向量。
```C++
int n; // 矩阵规模
double A[maxn][maxn]; // 用于存储A矩阵
double b[maxn]; // 用于存储b向量
double temp[maxn][maxn]; // 用于存储中间运算用的矩阵
double x[maxn]; // 用于存储答案的向量
```

### 数据输入与输出
读入矩阵
```C++
/*读取矩阵*/
void read() {
  cout << "Please input the scale of matrix n: ";
  cin >> n;
  cout << "|--------------------\n";
  cout << "|Please input the data: " << endl;

  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cin >> A[i][j];
    }
  }

  for (int i = 1; i <= n; i++) {
    cin >> b[i];
  }
}
```

每次消元后打印当前矩阵

```C++
/*显示中间步骤*/
void printTemp(int cases) {
  cout << "Times: " << cases << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << A[i][j] << ' ';
    }
    cout << "| " << b[i] << endl;
  }
  cout << "END THIS SHOW-------------------------" << endl;
}
```

最终输出x向量

```C++
/*显示结果*/
void print() {
  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}
```
注意：为了便于实现算法，矩阵和向量的存储**下标均从1开始**。

### 顺序消元法
&emsp;顺序消元法分为两个步骤：第一是**消元**，把原始矩阵变成一个上三角矩阵；第二是**回代**，通过最后一个方程一直回代解出整个矩阵。
```C++
/*顺序消元法*/
void normalElimination() {
  // Elimination
  for (int k = 1; k < n; k++) {
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= temp[i][k] * A[k][j];
      }
    }
    for (int i = k + 1; i <= n; i++) {
      b[i] -= temp[i][k] * b[k];
    }

    printTemp(k);
  }

  // 回代
  x[n] = b[n] / A[n][n];
  for (int i = n - 1; i >= 1; i--) {
    x[i] = b[i];
    for (int j = i + 1; j <= n; j++) {
      x[i] -= A[i][j] * x[j];
    }
    x[i] /= A[i][i];
  }

  print();
}
```

### 列主元消元法
&emsp;首先我要解释一下为什么会有这个方法。在顺序消元法中，我们看到了这样一个语句`temp[i][k] = A[i][k] / A[k][k];`，这是用来求出第i行与主元行的比值。注意到，如果某一个主元`A[k][k]`特别小的话，被一个相对大的数除，就会存在精度损失的问题。为了克服这个问题，我们希望在一列中，每一次都尽可能地挑选**绝对值最大**的一个元素作为主元，这就是**列主元消元法**。
&emsp;该方法的**消元**和**回代**两个主要步骤和**顺序消元法**是一致的，不同的地方在于多了**选主元**和**换行**这两步。

```C++
/*列主元消去法*/
void maximalColumnPivotElimination() {
  for (int k = 1; k < n; k++) {
    // 选主元
    double ab_max = - 1;
    int max_ik;
    for (int i = k; i <= n; i++) {
      if (fabs(A[i][k]) > ab_max) {
        ab_max = fabs(A[i][k]);
        max_ik = i;
      }
    }

    // 交换行处理(判断0矩阵)
    if (ab_max < e) {
      cout << "det A = 0\n";
      break;
    }
    else if (max_ik != k) {
      double temp;
      // 交换矩阵的第k行和第max_ik行
      for (int j = 1; j <= n; j++) {
        temp = A[max_ik][j];
        A[max_ik][j] = A[k][j];
        A[k][j] = temp;
      }
      // 交换b中的第k行和第max_ik行
      temp = b[max_ik];
      b[max_ik] = b[k];
      b[k] = temp;
    }

    // 消元计算
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= A[k][j] * temp[i][k];
      }
      b[i] -= temp[i][k] * b[k];
    }
    printTemp(k);
    if (k < n-1) {
      continue;
    }
    else {
      if (abs(A[n][n]) < e) {
        cout << "det A = 0\n";
        break;
      }
      else {
        x[n] = b[n] / A[n][n];
        for (int i = n - 1; i >= 1; i--) {
          x[i] = b[i];
          for (int j = i + 1; j <= n; j++) {
            x[i] -= A[i][j] * x[j];
          }
          x[i] /= A[i][i];
        }
        print();
      }
    }
  }
}
```

## 完整代码
```C++
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
#define e 0.00000001
#define maxn 50

int n;
double A[maxn][maxn];
double b[maxn];
double temp[maxn][maxn];
double x[maxn];

/*读取矩阵*/
void read() {
  cout << "Please input the scale of matrix n: ";
  cin >> n;
  cout << "|--------------------\n";
  cout << "|Please input the data: " << endl;

  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cin >> A[i][j];
    }
  }

  for (int i = 1; i <= n; i++) {
    cin >> b[i];
  }
}

/*显示中间步骤*/
void printTemp(int cases) {
  cout << "Times: " << cases << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << A[i][j] << ' ';
    }
    cout << "| " << b[i] << endl;
  }
  cout << "END THIS SHOW-------------------------" << endl;
}

/*显示结果*/
void print() {
  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}

/*顺序消元法*/
void normalElimination() {
  // Elimination
  for (int k = 1; k < n; k++) {
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= temp[i][k] * A[k][j];
      }
    }
    for (int i = k + 1; i <= n; i++) {
      b[i] -= temp[i][k] * b[k];
    }

    printTemp(k);
  }

  // 回代
  x[n] = b[n] / A[n][n];
  for (int i = n - 1; i >= 1; i--) {
    x[i] = b[i];
    for (int j = i + 1; j <= n; j++) {
      x[i] -= A[i][j] * x[j];
    }
    x[i] /= A[i][i];
  }

  print();
}

/*列主元消去法*/
void maximalColumnPivotElimination() {
  for (int k = 1; k < n; k++) {
    // 选主元
    double ab_max = - 1;
    int max_ik;
    for (int i = k; i <= n; i++) {
      if (fabs(A[i][k]) > ab_max) {
        ab_max = fabs(A[i][k]);
        max_ik = i;
      }
    }

    // 交换行处理(判断0矩阵)
    if (ab_max < e) {
      cout << "det A = 0\n";
      break;
    }
    else if (max_ik != k) {
      double temp;
      // 交换矩阵的第k行和第max_ik行
      for (int j = 1; j <= n; j++) {
        temp = A[max_ik][j];
        A[max_ik][j] = A[k][j];
        A[k][j] = temp;
      }
      // 交换b中的第k行和第max_ik行
      temp = b[max_ik];
      b[max_ik] = b[k];
      b[k] = temp;
    }

    // 消元计算
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= A[k][j] * temp[i][k];
      }
      b[i] -= temp[i][k] * b[k];
    }
    printTemp(k);
    if (k < n-1) {
      continue;
    }
    else {
      if (abs(A[n][n]) < e) {
        cout << "det A = 0\n";
        break;
      }
      else {
        x[n] = b[n] / A[n][n];
        for (int i = n - 1; i >= 1; i--) {
          x[i] = b[i];
          for (int j = i + 1; j <= n; j++) {
            x[i] -= A[i][j] * x[j];
          }
          x[i] /= A[i][i];
        }
        print();
      }
    }
  }
}

int main() {
  while (1) {
    read();
    //normalElimination();
    maximalColumnPivotElimination();
  }
}
```
高斯消元法的**计算复杂度**大约是**O($\frac{1}{3} n^3$)**
高斯消元法的代码实现讲解到这里结束了，谢谢！
