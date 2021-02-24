# From Word Embeddings To Document Distances

## Abstract

提出Word Mover's Distance(WMD), 一种用来度量两个文本档案之间距离的度量函数，可以用来计算文本相似度。

以预训练词向量为基础。

WMD 可以用来衡量两个文本文档之间的不相似性。

## Introduction

问题：如何判别两个文档中内容的距离？

可以应用在新闻分类聚类、歌曲识别、多文档匹配。

最简单的方法是构建一个bag of words(BOW)或term frequency-inverse document frequency(TF-IDF)，但这两种方法较难准确衡量出两个文档之间的距离，尤其是在同义不同词的情况。

而WMD有以下优点：

1. it is hyper-parameter free and straight-forward to understand and use.
2. it is highly interpretable as the distance between two documents can be broken down and explained as the sparse distances can be broken down and explained as the sparse distances between few individual words.
3. it naturally incorporates the knowledge encoded in the word2vec space and leads to high retrieval accuracy.



## Related Work

主要说了一下LSI、LDA、BOW、TF-IDF



## Word2Vec Embedding

说了一下当年Mikolov提出的Word2Vec。



## Word Mover's Distance

### nBOW representation

若采用Bag of Words系列的方法，会因为离散分布而导致无法评估近义词。

### Word travel cost

使用欧式距离衡量word2vec的embedding间两个词的差距。



## 基本思路

将文本以BOW的方式录入，再套用word2vec的词向量矩阵，获得录入文本的每个词的词向量。

在衡量两个文本的相似度的时候，计算两个文本的词向量距离



## Word Mover's Distance(WMD)处理流程

1. 去除stop words
2. 归一化词频，词频后续作为约束条件
3. 文档距离直观表示，将每个词之间交叉比对



## 补充资料

相关计算原理与细节建议参考这篇：https://zhuanlan.zhihu.com/p/145500656