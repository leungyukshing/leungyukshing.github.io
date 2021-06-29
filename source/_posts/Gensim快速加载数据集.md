---
title: Gensim快速加载数据集
abbrlink: 33599
date: 2020-01-03 21:55:52
tags:
---

## Gensim加载速度缓慢

&emsp;&emsp;[Gensim](https://radimrehurek.com/gensim/)是Python的一个专注于**主题建模**和**自然语言处理**的开源库，最近我在做embedding相关的研究用到了这个包。从网上下载了腾讯AI Lab提供的800w中文embedding语料数据集（[传送门](https://ai.tencent.com/ailab/nlp/embedding.html)），在加载到word2vec模型的时候非常缓慢。

<!-- more -->

本机配置：

+ CPU：Intel(R) Core(TM) i7-6600U 2.60Hz
+ RAM: 12G

## 解决方法

&emsp;&emsp;虽然笔记本的性能肯定是不足以运行这么庞大的数据集，但是也不是没有办法解决这个问题。我们从两个方面解决。

### 方法1：缩减数据集

&emsp;&emsp;原始的数据集解压后有将近16个G的大小，包含800万个中文词汇，但是在大多数场景下，尤其是实际应用场景下，我们用到的可能也就10万个左右，在加载的时候可以加上`limit`参数限制数量。我设置了`limit=10000`，加载的时间不超过2秒。

```python
from gensim.models import KeyedVectors

file = 'data/Tencent_AILab_ChineseEmbedding/Tencent_AILab_ChineseEmbedding.txt'
wv_from_text = KeyedVectors.load_word2vec_format(file, binary=False, limit=100000)
```

### 方法2：保存模型

&emsp;&emsp;使用语料库导入到word2vec模型实际上我们得到的是一个前向推理的神经网络，如果我们不训练只使用这个网络做推导，在后续的使用过程中该网络的参数是不会改变的，因此我们可以保存这些参数下来，一个网络中的参数的大小肯定是远小于整个语料库的大小。而且，word2vec模型直接加载参数肯定比从数据集中构建要快得多。我尝试保存了10万条数据的网络，大小不超过5MB。如果我们想用在实时应用，我们可以先在一台性能比较好的机器上将数据导入，保存好模型参数，然后把这个参数文件放到服务器上，服务启动的时候加载就可以了。

```python
from gensim.models import KeyedVectors

file = 'data/Tencent_AILab_ChineseEmbedding/Tencent_AILab_ChineseEmbedding.txt'
wv_from_text = KeyedVectors.load_word2vec_format(file, binary=False, limit=100000)
wv_from_text.init_sims(replace=True) # save memory to run faster

# save model for laster use
wv_from_text.save('./test.bin')

# load model from bin file
model = KeyedVectors.load('./test.bin')
```



&emsp;&emsp;以上就是两种Gensim加快数据导入的方法，希望能够帮助到你，谢谢！