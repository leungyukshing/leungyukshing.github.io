---
title: KNN算法
tags:
  - Machine Learning
mathjax: true
abbrlink: 39151
date: 2019-02-25 23:55:05
---

## Intro

&emsp;&emsp;**k-nearest neighbors algorithm**（KNN），k近邻算法，是机器学习中的一种常用的算法。它又称作是基于实例的学习（instance-based learning）或懒学习（lazy learning）。在这篇博客我将利用一个简单的应用来给大家介绍KNN。
<!-- more -->

## 原理

&emsp;&emsp;首先来看一下什么叫**instance-based learning**。一般的学习都是基于大量的数据，然后构建好一个模型，采用像梯度下降法来极小化代价函数，最终得出一个预测模型。在面对一个新输入的实例，使用这个模型给出预测。对于这种学习，大量的工作耗费在前面学习的过程中，而后面预测的时候就非常简单。而基于实例的学习却恰恰相反，它几乎不存在任何学习或训练的步骤。仅当它接受一个新的输入实例时，它才开始真正的工作，这也是它被成为懒学习的原因。

&emsp;&emsp;然后我们来看什么叫k-近邻。在这之前，先来理解一个很重要的概念——**欧氏距离**。对于$n$维空间而言，其欧氏距离为：
$$
d(x, y) = \sqrt{(x_1-y_1)^2+(x_2-y_2)^2+\dots+(x_n-y_n)^2} = \sqrt{\sum^n_{i=1}(x_i-y_i)^2}
$$
&emsp;&emsp;欧氏距离给我们定义了在$n$维空间中两个点的距离。幸运的是，我们几乎可以使用类似的方式来表示每一个实例。每一个实例可以由许多特征值描述，这些特征值就是其空间坐标，所以每一个实例实质上就是在多维空间中的一个点。

&emsp;&emsp;接着我们来看看学习的过程是怎样的。学习其中一个很基本的思想就是：**物以类聚，人以群分**。具有相似特征的东西一般都具有相似的性质。因此结合上面的欧氏距离，k-近邻算法就是为我们提供了这样一个学习方法：对于一个新实例$X$，该算法在空间中找出离它最近的$k$的点（这些点是训练集的实例），在这些点之中进行投票，投票数最多的类别就认为是这个$X$的类别。

&emsp;&emsp;所以**k-近邻算法的开销都在预测的过程**，在分类问题中应用效果比较好，当然其缺点也是存在的。如果实例使用非常多的特征表示，而与他们性质有关的特征可能只有几个，这样使用k-近邻算法的效果就会很差。例如，如果我们要对学生进行分类，分为“适合运动”和“不适合运动”，但是如果描述一个学生的特征包括了身高、体重、各科成绩等，对分类将会有很大影响（因为成绩在这里是干扰因素，但是有很多科成绩，所以身高体重相近的人可能因为成绩差异大而被分为两类）。这也说明在使用k-近邻算法前可能要先提取重要特征。

## 实战

&emsp;&emsp;这里使用KNN实现一个简单的天气预测模型。其基本原理是：给出用户数据【吃的冰淇淋数量，喝水数量，室外活动时间】，预测出当前的天气。为了更好地理解KNN的原理，这里是自己实现KNN算法，后面也会给出使用sklearn包实现的完整代码。

&emsp;&emsp;为了简便，这里天气只有两类“非常热”和“一般热”。数据集也是手动构建少量数据。

### 构建数据集

&emsp;&emsp;这里先手动构建一个简单的数据集，包含数据和标签值。数据集最好大于4，因为后面如果用sklearn的时候数据集要求大于4。

```python
'''
  create dataset
'''
def create_dataset():
  datasets = np.array([[8, 4, 2], [7, 1, 1], [1, 4, 4], [3, 0, 5], [9, 4, 2], [7, 0, 1], [1, 5, 4], [4, 0 ,5]]) # dataset
  labels = [0, 0, 1, 1, 0, 0, 1, 1] # label
  return datasets, labels
```

### 计算欧氏距离

&emsp;&emsp;这是整个KNN算法的核心。我们可以直接将上面写的公式转化为代码，如下：

```python
def computeDistance(v1, v2, length):
  distance = 0
  for x in range(length):
    distance += pow((v1[x] - v2[x]), 2)
  return math.sqrt(distance)
```

&emsp;&emsp;但是实际应用的时候就会发现，我们一般计算的都不只是两个实例之间的距离，而是一个实例与整个训练集各个点的距离，因此我们可以直接将这个过程封装好。将训练集中每个实例与新实例相减，然后求平方，最后求和再开根号，得到的还是一个矩阵。

```pyth
def computeEuclideanDistance(newV, datasets):
  rowsize, colsize = datasets.shape
  diffMat = tile(newV,(rowsize, 1)) - datasets
  sqDiffMat = diffMat**2
  sqrtDiffMat = sqDiffMat.sum(axis=1)**0.5
  return sqrtDiffMat
```

### 构建模型

&emsp;&emsp;我们将得到的距离矩阵排序，目的是得到离新实例最近的k个点（k个邻居），然后进行投票。投票过程实质上是在维护一个键值对集合。最后输出这个集合投票数最多的key就可以了。

```python
'''
  Construct KNN
'''
def knn_Classifier(newV, datasets, labels, k):
  # Calculate the distance
  distance = computeEuclideanDistance(newV, datasets)
  #print(distance)

  # Sort
  sortedDistanceIndex = distance.argsort(axis=0)
  #print(sortedDistanceIndex)

  # Count
  classCount = {} # pair(label, count)
  for i in range(k):
    votelabels = labels[sortedDistanceIndex[i]]
    classCount[votelabels] = classCount.get(votelabels, 0) + 1

  # Vote(Descending Order)
  sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=1)
  #print(sortedClassCount)

  # Print
  #print(newV, 'KNN predict: ', sortedClassCount[0][0])
  return sortedClassCount[0][0]
```

### 完整代码

&emsp;&emsp;然后写一个测试用的函数进行简单的测试，最后添加用户输入提示，支持对话式查询的方法来预测，封装好模型。

```python
# coding=utf8
'''
  Author: LeungYukshing
'''

import numpy as np
from numpy import *

import matplotlib
import matplotlib.pyplot as plt

import math
import operator

'''
  create dataset
'''
def create_dataset():
  datasets = np.array([[8, 4, 2], [7, 1, 1], [1, 4, 4], [3, 0, 5], [9, 4, 2], [7, 0, 1], [1, 5, 4], [4, 0 ,5]]) # dataset
  labels = [0, 0, 1, 1, 0, 0, 1, 1] # label
  return datasets, labels


'''
  Construct KNN
'''
def knn_Classifier(newV, datasets, labels, k):
  # Calculate the distance
  distance = computeEuclideanDistance(newV, datasets)
  #print(distance)

  # Sort
  sortedDistanceIndex = distance.argsort(axis=0)
  #print(sortedDistanceIndex)

  # Count
  classCount = {} # pair(label, count)
  for i in range(k):
    votelabels = labels[sortedDistanceIndex[i]]
    classCount[votelabels] = classCount.get(votelabels, 0) + 1

  # Vote(Descending Order)
  sortedClassCount = sorted(classCount.items(), key=operator.itemgetter(1), reverse=1)
  #print(sortedClassCount)

  # Print
  #print(newV, 'KNN predict: ', sortedClassCount[0][0])
  return sortedClassCount[0][0]


'''
  Compute Euclidean Distance
'''
def computeDistance(v1, v2, length):
  distance = 0
  for x in range(length):
    distance += pow((v1[x] - v2[x]), 2)
  return math.sqrt(distance)

def computeEuclideanDistance(newV, datasets):
  rowsize, colsize = datasets.shape
  diffMat = tile(newV,(rowsize, 1)) - datasets
  sqDiffMat = diffMat**2
  sqrtDiffMat = sqDiffMat.sum(axis=1)**0.5
  return sqrtDiffMat

'''
  Encapsulation
'''
def predictModel():
  datasets, labels = create_dataset()
  # print('Dataset:\n', datasets, '\nLabel:\n', labels)

  # Prompt the user to input
  iceCream = float(input('How many ice cream did you have today? '))
  water = float(input('How many bottles of water did you drink today? '))
  play = float(input('How long did you spend outdoor? '))

  newV = np.array([iceCream, water, play])
  res = knn_Classifier(newV, datasets, labels, 3)
  print('\nYou may feel: ', '一般热'if res==1 else '非常热')

'''
  Test Function
'''
def test():
  # KNN Test
  print('\nSingle Test:')
  newV = [2, 4, 4]
  result = knn_Classifier(newV, datasets, labels, 3)
  print('\n', newV, 'KNN predict: ', result)

  print('\n\nMulti Test:')
  vecs = array([[2, 4, 4], [3, 0, 0], [5, 7, 2]])
  for vec in vecs:
    vecsRes = knn_Classifier(vec, datasets, labels, 3)
    print('\n', vec, 'KNN predict: ', vecsRes)


if __name__ == '__main__':
  # KNN App
  predictModel()
```

### sklearn实现

&emsp;&emsp;KNN作为一种著名的机器学习分类算法，在sklearn中有封装好的函数可以直接使用。

```python
#coding = utf8
'''
  Author: LeungYukshing
'''

from sklearn import neighbors
from numpy import *
import knn as KNN

def knn_sklearn_predict():
  knn = neighbors.KNeighborsClassifier()
  datasets, labels = KNN.create_dataset()

  knn.fit(datasets, labels)

  # Prompt the user to input
  iceCream = float(input('How many ice cream did you have today? '))
  water = float(input('How many bottles of water did you drink today? '))
  play = float(input('How long did you spend outdoor? '))

  newV = array([iceCream, water, play])

  res = knn.predict([newV])
  print('You may feel: ','非常热' if res[0] == 0 else '一般热')
  return res

if __name__ == '__main__':
  knn_sklearn_predict()
```

### 结果

![天气预测](/images/天气预测.png)

## 小结

&emsp;&emsp;KNN算法是比较简单的机器学习算法，其中包含的数学原理并不多。通过这个简单的例子应该能够很好地理解KNN的工作原理。希望这篇博客能够帮助到你，谢谢您的支持！
