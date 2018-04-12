---
title: LU分解
mathjax: true
abbrlink: 32612
date: 2018-04-02 13:23:52
tags:
---

## 前言
&emsp;将[高斯消元法](https://leungyukshing.github.io/archives/GaussianElimination.html)改写为紧凑形式，可以直接从矩阵**A**的元素的得到计算**L**，**U**元素的递推公式，而不需要任何中间步骤，这就是直接三角分解法。这样我们就可以把求解$Ax = b$的问题转化成求解两个三角形方程组的问题：
（1）**Ly = b**，求**y**；
（2）**Ux = y**，求**x**。
<!-- more -->

## 算法分析
对于一个非奇异矩阵**A**， 有分解式
$$
A = LU
$$
，其中**L**为单位下三角矩阵，**U**为上三角矩阵，即
$$
    A=
    \begin{bmatrix}
    1 \\
    l_{21} & 1 \\
    \vdots & \vdots & \ddots \\
    l_{n1} & l_{n2} & \cdots & 1\\
    \end{bmatrix}
    \begin{bmatrix}
    u_{11} & u_{12} & \cdots & u_{1n} \\
     & u_{22} & \cdots & u_{2n} \\
     & & \ddots & \vdots \\
     & & & u_{nn}\\
    \end{bmatrix}
$$
上面只是我们的猜想，现在我们就来说明**L**，**U**的元素可以由$n$步直接计算定出，其中第$r$步定出**U**的第$r$行和**L**的第$r$列元素：
$$
a_{1i} = u_{1i}, i=1,2,...,n
$$
得**U**的第1行元素；
$$
a_{i1} = l_{i1}u_{11}, l_{i1} = a_{i1} / u_{i1}, i=2,3,...,n
$$
得**L**的第1列元素。

设已经定出**U**的第1行到第$r-1$行元素与**L**的第$1$列到第$r-1$列元素，由分解式得到：
$$
a_{ri} = \sum_{k=1}^nl_{rk}u_{ki} = \sum_{k=1}^{r-1}l_{rk}u_{ki} + u_{ri},
$$
故
$$
u_{ri} = a_{ri} - \sum_{k=1}^{r-1}, i=r,r+1,...,n
$$
所以，
$$
a_{ir} = \sum_{k=1}^nl_{ik}u_{kr} = \sum_{k=1}^{k-1}l_{ik}x_{kr} + l_{ir}u_{rr}
$$

综上所述，分解的计算公式为：
$$① u_{1i} = a_{1i}, l_{i1} = a_{i1} / u_{11}, i=2,3,...,n$$
$$② u_{ri} = a_{ri} - \sum_{k=1}^{r-1}l_{rk}u_{ki}, i=r,r+1,...,n$$
$$③ l_{ir} = (a_{ir} - \sum_{k=1}^{r-1}l_{ik}u_{kr} / u_{rr}, i=r+1,...,n, 且r≠n
.$$

所以求解**Ly=b**和**Ux = y**的计算公式为：
$$
\begin{cases}
y_1 = b_1\\
y_i = b_i - \sum_{k=1}^{i-1}l_{ik}y_k, i=2,3,...,n;\\
\end{cases}
$$
$$
\begin{cases}
x_n = y_n / u_{nn}\\
x_i = (y_i - \sum_{k=i+1}^{n}l_{ik}x_k) / u_{ii}, i=n-1, n-2,...,1\\
\end{cases}
$$

## 代码分析
### 变量定义
```C++
int n;
double A[maxn][maxn] = {0};
double L[maxn][maxn] = {0};
double U[maxn][maxn] = {0};
double b[maxn] = {0};
double y[maxn] = {0};
double x[maxn] = {0};
```
使用二维数组存储矩阵，使用一维数组存储向量。

### 输入与输出
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

/*打印矩阵*/
void printLU() {
  cout << "L matrix: " << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout <<  setw(10) << L[i][j] << " ";
    }
    cout << endl;
  }

  cout << "U matrix: " << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << U[i][j] << " ";
    }
    cout << endl;
  }
}

/*显示结果*/
void print() {
  cout << "|-------------------" << endl;
  for (int i = 1; i <= n; i++) {
    cout << "y[" << i << "] = " << y[i] << endl;
  }
  cout << "|-------------------" << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << endl;
  }
}
```
分别有处理输入、输出分解后结果和输出最终解的三个方法。

### LU分解
```C++
void LUDecomposition() {
  int i, r, k;
  // 进行U的第一行的赋值
  for (i = 1; i <= n; i++) {
    U[1][i] = A[1][i];
  }

  // 进行L的第一列的赋值
  for (i = 2; i <= n; i++) {
    L[i][1] = A[i][1] / U[1][1];
  }

  // 计算U剩下的行数和L剩下的列数(分解过程)
  for (r = 2; r <= n; r++) {
    for (i = r; i <= n; i++) {
      double sum1 = 0;
      for (k = 1; k < r; k++) {
        sum1 += L[r][k] * U[k][i];
      }
      U[r][i] = A[r][i] - sum1;
    }

    if (r != n) {
      for (i = r + 1; i <= n; i++) {
        double sum2 = 0;
        for (k = 1; k < r; k++) {
          sum2 += L[i][k] * U[k][r];
        }
        L[i][r] = (A[i][r] - sum2) / U[r][r];
      }
    }
  }
  for (int i = 1; i <= n; i++) {
    L[i][i] = 1;
  }

  /*回代求解*/
  y[1] = b[1];
  for (i = 2; i <= n; i++) {
    double sum3 = 0;
    for (k = 1; k < i; k++) {
      sum3 += L[i][k] * y[k];
    }
    y[i] = b[i] - sum3;
  }


  x[n] = y[n] / U[n][n];
  for (i = n - 1; i >= 1; i--) {
    double sum4 = 0;
    for (k = i + 1; k <= n; k++) {
      sum4 += U[i][k] * x[k];
    }
    x[i] = (y[i] - sum4) / U[i][i];
  }
  printLU();
  print();
}
```
按照上述的算法进行编程，最后输出LU矩阵和x。
### 完整代码
```C++
#include <iostream>
#include <iomanip>
using namespace std;
#define maxn 50

int n;
double A[maxn][maxn] = {0};
double L[maxn][maxn] = {0};
double U[maxn][maxn] = {0};
double b[maxn] = {0};
double y[maxn] = {0};
double x[maxn] = {0};

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

/*打印矩阵*/
void printLU() {
  cout << "L matrix: " << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout <<  setw(10) << L[i][j] << " ";
    }
    cout << endl;
  }

  cout << "U matrix: " << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << U[i][j] << " ";
    }
    cout << endl;
  }
}

/*显示结果*/
void print() {
  cout << "|-------------------" << endl;
  for (int i = 1; i <= n; i++) {
    cout << "y[" << i << "] = " << y[i] << endl;
  }
  cout << "|-------------------" << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << endl;
  }
}



void LUDecomposition() {
  int i, r, k;
  // 进行U的第一行的赋值
  for (i = 1; i <= n; i++) {
    U[1][i] = A[1][i];
  }

  // 进行L的第一列的赋值
  for (i = 2; i <= n; i++) {
    L[i][1] = A[i][1] / U[1][1];
  }

  // 计算U剩下的行数和L剩下的列数(分解过程)
  for (r = 2; r <= n; r++) {
    for (i = r; i <= n; i++) {
      double sum1 = 0;
      for (k = 1; k < r; k++) {
        sum1 += L[r][k] * U[k][i];
      }
      U[r][i] = A[r][i] - sum1;
    }

    if (r != n) {
      for (i = r + 1; i <= n; i++) {
        double sum2 = 0;
        for (k = 1; k < r; k++) {
          sum2 += L[i][k] * U[k][r];
        }
        L[i][r] = (A[i][r] - sum2) / U[r][r];
      }
    }
  }
  for (int i = 1; i <= n; i++) {
    L[i][i] = 1;
  }

  /*回代求解*/
  y[1] = b[1];
  for (i = 2; i <= n; i++) {
    double sum3 = 0;
    for (k = 1; k < i; k++) {
      sum3 += L[i][k] * y[k];
    }
    y[i] = b[i] - sum3;
  }


  x[n] = y[n] / U[n][n];
  for (i = n - 1; i >= 1; i--) {
    double sum4 = 0;
    for (k = i + 1; k <= n; k++) {
      sum4 += U[i][k] * x[k];
    }
    x[i] = (y[i] - sum4) / U[i][i];
  }
  printLU();
  print();
}
int main() {
  while (1) {
    read();
    LUDecomposition();
  }
}
```
LU分解法求解$Ax = b$的**算法复杂度**大约是**O($\frac{1}{3} n^3$)**，但是如果对于不同**b**来说，一旦完成了分解，之后的每一次求解复杂度仅为**O($n^2$)**，这是LU分解相比起高斯消元法有优势的地方。

至此，矩阵的LU分解算法及代码的讲解已经结束了，谢谢！

---
#### 参考资料：
1.数值分析（第5版）  李庆扬，王能超，易大义 *编
