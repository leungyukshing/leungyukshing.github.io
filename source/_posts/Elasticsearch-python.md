---
title: ElasticSearch--Python接入
abbrlink: 40974
date: 2020-04-02 00:17:19
tags:
---

## ElasticSearch--Python接入

&emsp;&emsp; 之前介绍过文档搜索引擎ElasticSearch（[传送门](http://leungyukshing.cn/archives/Elasticsearch-basis.html)），今天我就来分享一下如何把使用python接入ES，进而作为我们的flask服务器的搜索引擎。

<!-- more -->

## 环境搭建与介绍

&emsp;&emsp;为了简单起见，本文所用到的ES环境由docker提供。

1. 首先抓取docker镜像。

   ```bash
   docker pull elasticsearch:6.8.7
   ```

2. 运行docker。

   ```bash
   docker run -d --name es -p 9200:9200 -p 9300:9300 elasticsearch:6.8.7
   ```

3. 安装kibana做监控(Optional)

   不同环境下安装方式不同。

   MacOS: `brew install kibana`

4. 运行kibana

   ```bash
   kibana
   ```

   然后打开http://loclahost:5601就能看到面板。

------

## Python接入

&emsp;&emsp;我们先从基本的Python接入开始介绍，主要覆盖一些简单的CURD操作。

1. 安装ES插件包。

   ```bash
   pip install elasticsearch
   ```

2. 引入ES插件包，并创建实例。

   ```python
   from elasticsearch import Elasticsearch
   es = Elasticsearch()
   ```

3. 新增及插入。

   主要是使用`index()`方法，参数有`index`（索引名，相当于数据库名）、`doc_type`（文档类型，相当于表名）、`body`（内容，相当于一条record）。

   ```python
   body1 = {
       'name': 'test hello',
       'age': 32,
       'school': 'scnu'
   }
   
   es.index(index="qii_index", doc_type="table1", body=body1)
   ```

   这里我插入了多条数据用于后面的搜索演示，在Kibana上就能够看到内容。

   ![kibana](/images/es_python3.jpg)

4. 搜索。

   主要是使用`search()`方法，里面可以搜索参数等。同样地，调用时需要指定`index`和`doc_type`。

   获取所有结果

   ```python
   result = es.search(index="qii_index", doc_type="table1") // search all result
   
   for item in result["hits"]["hits"]:
       print(item["_source"])
   ```

   ![search all result](/images/es_python0.jpg)

   按照名字搜索

   ```python
   _query_name_contains = {
       'query': {
           'match': {
               'name': 'test'
           }
       }
   }
   
   result2 = es.search(index="qii_index", doc_type="table1", body=_query_name_contains)
   
   for item in result["hits"]["hits"]:
       print(item["_source"])
   ```

   ![search by name](/images/es_python1.jpg)

   这里的搜索结果可以分析一下，我们搜索的是`test`。匹配到的结果是`test`和`test hello`，而`test2`则没有匹配上，说明这个分词器对于英文而言是使用空格切分，匹配是以单词为单位的。我们也可以去验证一下中文的效果。打开Kibana，在Dev Tools中输入以下内容，就能看到分词器是如何去分解一个查询词的。

   ![es tokenizer](/images/es_python2.jpg)

   可以看到对于中文来说，是逐个字分开的。关于分词的优化后面会继续讨论。

---

## Flask接入

TBC...

---

## Reference

1. [ES 搭建及使用](https://qii404.me/2018/11/22/elastic-search.html)