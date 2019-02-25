---
title: LFM--电影推荐系统应用
tags:
  - Machine Learning
mathjax: true
abbrlink: 6791
date: 2019-02-24 23:02:19
---

## Intro

&emsp;&emsp;这篇博客就向大家介绍一下常用的推荐算法——LFM。LFM（Latent Factor Model），隐语义模型，是一种非常著名的推荐系统算法，它在NetFlix的推荐算法竞赛中获奖，最早被应用于电影推荐中。这里我将会简单介绍其原理，然后实现一个简单的电影推荐系统。
<!-- more -->

---

## 原理

&emsp;&emsp;先看看LFM的基本思想：每个用户（user）都有自己对电影（movie）的偏好。我们认为一部电影带有一些latent factor（如动作片、科幻片、爱情片、纪录片等）。所以我们就有以下两条假设：

+ 不同的用户对于不同的factor有不同的偏好程度。矩阵Q：

|       | 动作 | 爱情 | 科幻 |
| :---: | :--: | :--: | :--: |
| 用户1 | 0.6  | 0.1  | 0.2  |
| 用户2 | 0.2  | 0.8  | 0.3  |

+ 不同的电影含有的factor的比重是不同的。矩阵P：

|       | 动作 | 爱情 | 科幻 |
| :---: | :--: | :--: | :--: |
| 电影A | 0.9  | 0.4  | 0.1  |
| 电影B | 0.2  | 0.2  | 0.8  |

&emsp;&emsp;根据以上两个矩阵，我们就可以知道用户1对电影A的喜好程度为：
$$
0.6\*0.9 + 0.1\*0.4+0.2\*0.1 = 0.6
$$
&emsp;&emsp;对电影B的喜好程度为：
$$
0.6\*0.2+0.1\*0.2+0.2\*0.8 = 0.3
$$
&emsp;&emsp;所以我们可以很直观地看出，用户1更喜换电影A。根据这种思路，我们可以计算出每个用户对于每首歌的评分（当然这个评分是我们估计的）。评分矩阵$\hat{R}$可以这样计算：
$$
\hat{R}=QP^T
$$
&emsp;&emsp;现在估计的问题已经解决，但是LFM最大的问题在于我们如何获得Q和P。在实际应用中，让用户告诉我们对每个类型的偏好系数是不现实的，我们一般获取到的是用户行为，因此我们需要自己构建出一个评分矩阵$R$。我们可以自定义一些规则来将用户行为转变成为评分，如：

+ 看完整部电影 = 4
+ 分享给别人 = 3
+ 收藏 = 2
+ 搜索该电影名字 = 1
+ 只观看10分钟 = -2

&emsp;&emsp;这样我们就可以构建出一个实际的评分矩阵$R$。事实上这是一个非常高维的稀疏矩阵，因为绝大多数的用户都是只看过一小部分的电影。我们可以利用稀疏矩阵这个特性，对$R$作UV分解。也就是说，我们把构建$R$的问题转化为了用$Q$和$P$两个矩阵的乘积去得到$\hat{R}$。这是一个近似的问题，因此就得到了可优化的代价函数：
$$
J(\theta) = \sum(r_{ui} - q_ip^T_u)^2
$$
&emsp;&emsp;这个就是我们用于优化求解的代价函数。在实际应用中，往往还会在后面加上2范数的罚相$\lambda\parallel p_u \parallel^2 + \lambda\parallel q_i \parallel^2$。最常用的方法就是使用梯度下降法来求解$P$和$Q$。经过学习后，我们就能够得到一个比较理想的$\hat{R}$，然后用这个评分矩阵去估计某一个用户对于某一个电影的感兴趣程度。

---

## 实战

&emsp;&emsp;下面我将实现一个简单的电影推荐系统。首先获取数据集：[传送门](https://grouplens.org/datasets/movielens/)。在**recommended for education and development**中下载*Small*数据集。

![电影数据](/images/电影数据.png)

&emsp;&emsp;下载并解压后其实我们可以看到，里面包含了几个csv数据，我们主要用到两个数据`ratings.csv`（用户对自己看过的电影的评分）和`movies.csv`（电影信息）。

**注意**：这里的数据是定期更新的，并且官方不会保留历史版本，因此大家实战的时候可能与我的结果不一样。

### 数据预处理

&emsp;&emsp;首先对原始数据进行预处理，主要是将csv读入，然后提取表格中有用的信息。这里有两个特别要注意的：

+ 我们使用索引号（index）来替代`movieId`。这是因为`movieId`很大，构建出来的矩阵会很大，使用索引号替代`movieId`能提高效率。这一步要做的就是在`movies`表中建立`movieId`与`index`的对应关系，然后将这个表格合并到`ratings`表中（根据相同的`movieId`）。
+ 及时保存处理后的数据。这是在数据处理中一个很好的习惯，因为数据处理需要耗费大量的时间，所以一旦处理后我们应该保存起来。

Code：

```python
# Read Data
ratings_df = pd.read_csv('ml-latest-small/ratings.csv')

ratings_df.tail()

movies_df = pd.read_csv('ml-latest-small/movies.csv')

movies_df.tail()


# Add a new column, use columnNum to take the place of MovieId
movies_df['movieRow'] = movies_df.index

movies_df.tail()

# extract the useful information in the table
movies_df = movies_df[['movieRow', 'movieId', 'title']]

# save the processed data
movies_df.to_csv('moviesProcessed.csv', index=False, header=True, encoding='utf-8')

movies_df.tail()

# Merge two table(ratings_df, movies_df)
ratings_df = pd.merge(ratings_df, movies_df, on='movieId')

ratings_df.head()

# extract the useful information in the table
ratings_df = ratings_df[['userId', 'movieRow', 'rating']]

# save the processed data
ratings_df.to_csv('ratingProcessed.csv', index=False, header=True, encoding='utf-8')

ratings_df.head()
```

### 创建rating矩阵和record矩阵

&emsp;&emsp;我们先来看看这两个矩阵是什么。

+ rating矩阵就是我们之前提到的$R$评分矩阵，它是用户对电影的真实评分，是基于用户数据我们计算出来的。
+ record矩阵是记录用户评分记录的矩阵。如果用户$i$与电影$j$有评分记录，那么$record(i,j) = 1$。

Code:

```python
## 创建电影评分矩阵rating和评分记录矩阵record

# User Number
userNo = ratings_df['userId'].max() + 1
# Movie Number
movieNo = ratings_df['movieRow'].max() + 1

userNo

movieNo

# Initialize the rating matrix
rating = np.zeros((movieNo, userNo))

flag = 0

ratings_df_length = np.shape(ratings_df)[0]

for index, row in ratings_df.iterrows():
    rating[int(row['movieRow']), int(row['userId'])] = row['rating']
    flag += 1
    print('processed %d, %d left' % (flag, ratings_df_length - flag))

# Initialize the record matrix
record = rating > 0

record

# Convert from boolean to int
record = np.array(record, dtype=int)

record
```

### 构建学习模型

&emsp;&emsp;首先是对数据进行一个标准化处理。这是数据处理经常用的一个步骤，它可以提高数据处理的速度和精度。一般来说可以用sklearn来实现，但这里比较简单，不妨自己写一下。核心的思路就是：**原来的评分-平均评分=标准化后评分**。这样处理后每个评分都集中在一个区间中。

&emsp;&emsp;然后就使用tensorflow构建学习模型。我们随机初始化$Q$和$P$两个矩阵（使用正态分布）。然后定义好损失函数、优化器等。

Code:

```python
## 构建模型
def normalizeRatings(rating, record):
    m, n = rating.shape
    rating_mean = np.zeros((m, 1))
    rating_norm = np.zeros((m, n))
    for i in range(m):
        idx = record[i, :] != 0
        rating_mean[i] = np.mean(rating[i, idx])
        rating_norm[i, idx] -= rating_mean[i]
    return rating_norm, rating_mean

rating_norm, rating_mean = normalizeRatings(rating, record)

rating_norm = np.nan_to_num(rating_norm)

rating_norm

rating_mean = np.nan_to_num(rating_mean)

rating_mean


# Features to describe a movie
num_features = 10

X_parameter = tf.Variable(tf.random_normal([movieNo, num_features], stddev=0.35))

Theta_parameter = tf.Variable(tf.random_normal([userNo, num_features], stddev=0.35))

# Loss Function
loss = 1/2 * tf.reduce_sum(((tf.matmul(X_parameter, Theta_parameter, transpose_b=True) - rating_norm)*record)**2) + 1/2 * (tf.reduce_sum(X_parameter**2) + tf.reduce_sum(Theta_parameter**2))

optimizer = tf.train.AdamOptimizer(1e-4)
train = optimizer.minimize(loss)
```

### 训练模型

&emsp;&emsp;训练模型的部分主要是tensorflow的操作，这里就不仔细讲解了。我们可以使用tensorboard可视化工具来看到训练过程。

![训练过程](/images/训练过程.png)

&emsp;&emsp;可以看出损失函数随着训练次数的增加在下降。

Code:

```python
## 训练模型
tf.summary.scalar('loss', loss)

summaryMerged = tf.summary.merge_all()

filename = './movie_tensorboard'

writer = tf.summary.FileWriter(filename)

sess = tf.Session()

init = tf.global_variables_initializer()

sess.run(init)

for i in range(5000):
    _, movie_summary = sess.run([train, summaryMerged])
    writer.add_summary(movie_summary, i)
```

### 评估模型

&emsp;&emsp;从训练结果中获取到用于预测的模型（实质上就是一个预测的评分矩阵$\hat{R}$,$\hat{R}=QP^T$）。同时我们可以输出一下误差。

Code:

```python
# 评估模型
Current_X_parameters, Current_Theta_parameters = sess.run([X_parameter, Theta_parameter])

predicts = np.dot(Current_X_parameters, Current_Theta_parameters.T) + rating_mean

errors = np.sqrt(np.sum((predicts - rating)**2))

errors
```

### 测试

&emsp;&emsp;添加用户输入提示，获取一个用户Id，在$\hat{R}$中将该列提取出来并且排序，将评分排名前20的电影信息输出。

Code:

```python
# 构建完整的电影推荐系统
user_id = input('您要向哪位用户进行推荐？请输入用户编号：')

sortedResult = predicts[:, int(user_id)].argsort()[::-1]

idx = 0
print('为该用户推荐的评分最高的20部电影是：'.center(80, '='))
for i in sortedResult:
    print('评分：%.2f，电影名：%s' % (predicts[i, int(user_id)], movies_df.iloc[i]['title']))
    idx += 1
    if idx == 20: break

```

![推荐结果](/images/推荐结果.png)

&emsp;&emsp;这样，我们就成功构建起一个简单的电影推荐系统了。

### 完整代码

```python
# coding: utf-8
# Author: LeungYukshing

import pandas as pd
import numpy as np
import tensorflow as tf

# Read Data
ratings_df = pd.read_csv('ml-latest-small/ratings.csv')

ratings_df.tail()

movies_df = pd.read_csv('ml-latest-small/movies.csv')

movies_df.tail()


# Add a new column, use columnNum to take the place of MovieId
movies_df['movieRow'] = movies_df.index

movies_df.tail()

# extract the useful information in the table
movies_df = movies_df[['movieRow', 'movieId', 'title']]

# save the processed data
movies_df.to_csv('moviesProcessed.csv', index=False, header=True, encoding='utf-8')

movies_df.tail()

# Merge two table(ratings_df, movies_df)
ratings_df = pd.merge(ratings_df, movies_df, on='movieId')

ratings_df.head()

# extract the useful information in the table
ratings_df = ratings_df[['userId', 'movieRow', 'rating']]

# save the processed data
ratings_df.to_csv('ratingProcessed.csv', index=False, header=True, encoding='utf-8')

ratings_df.head()


## 创建电影评分矩阵rating和评分记录矩阵record

# User Number
userNo = ratings_df['userId'].max() + 1
# Movie Number
movieNo = ratings_df['movieRow'].max() + 1

userNo

movieNo

# Initialize the rating matrix
rating = np.zeros((movieNo, userNo))

flag = 0

ratings_df_length = np.shape(ratings_df)[0]

for index, row in ratings_df.iterrows():
    rating[int(row['movieRow']), int(row['userId'])] = row['rating']
    flag += 1
    print('processed %d, %d left' % (flag, ratings_df_length - flag))

# Initialize the record matrix
record = rating > 0

record

# Convert from boolean to int
record = np.array(record, dtype=int)

record

## 构建模型
def normalizeRatings(rating, record):
    m, n = rating.shape
    rating_mean = np.zeros((m, 1))
    rating_norm = np.zeros((m, n))
    for i in range(m):
        idx = record[i, :] != 0
        rating_mean[i] = np.mean(rating[i, idx])
        rating_norm[i, idx] -= rating_mean[i]
    return rating_norm, rating_mean

rating_norm, rating_mean = normalizeRatings(rating, record)

rating_norm = np.nan_to_num(rating_norm)

rating_norm

rating_mean = np.nan_to_num(rating_mean)

rating_mean


# Features to describe a movie
num_features = 10

X_parameter = tf.Variable(tf.random_normal([movieNo, num_features], stddev=0.35))

Theta_parameter = tf.Variable(tf.random_normal([userNo, num_features], stddev=0.35))

# Loss Function
loss = 1/2 * tf.reduce_sum(((tf.matmul(X_parameter, Theta_parameter, transpose_b=True) - rating_norm)*record)**2) + 1/2 * (tf.reduce_sum(X_parameter**2) + tf.reduce_sum(Theta_parameter**2))

optimizer = tf.train.AdamOptimizer(1e-4)
train = optimizer.minimize(loss)


## 训练模型
tf.summary.scalar('loss', loss)

summaryMerged = tf.summary.merge_all()

filename = './movie_tensorboard'

writer = tf.summary.FileWriter(filename)

sess = tf.Session()

init = tf.global_variables_initializer()

sess.run(init)

for i in range(5000):
    _, movie_summary = sess.run([train, summaryMerged])
    writer.add_summary(movie_summary, i)

# 评估模型
Current_X_parameters, Current_Theta_parameters = sess.run([X_parameter, Theta_parameter])

predicts = np.dot(Current_X_parameters, Current_Theta_parameters.T) + rating_mean

errors = np.sqrt(np.sum((predicts - rating)**2))

errors

# 构建完整的电影推荐系统
user_id = input('您要向哪位用户进行推荐？请输入用户编号：')

sortedResult = predicts[:, int(user_id)].argsort()[::-1]

idx = 0
print('为该用户推荐的评分最高的20部电影是：'.center(80, '='))
for i in sortedResult:
    print('评分：%.2f，电影名：%s' % (predicts[i, int(user_id)], movies_df.iloc[i]['title']))
    idx += 1
    if idx == 20: break
```

---

## 总结

&emsp;&emsp;这里通过构建一个简单的电影推荐系统来帮助理解LFM。其实质是矩阵的UV分解和机器学习优化参数。谢谢！
