# Sentence-BERT: Sentence Embeddings using Siamese BERT-Network

## Abstract

在做文本语义相似度判别任务中，BERT系的计算相当耗时，在10000个句子中找到最相似的一对约要50 million inference computations计算65个小时。使得BERT系的模型难以在文本相似度搜索上有较好的表现。

通过本文提出的Sentence BERT模型可以大幅降低计算最相似对的计算时间自65个小时至5秒，同时维持BERT的准确率。并通过本文模型在Semantic Textual Similarity（STS）

## Introduction

主要说了一下使用基于BERT的孪生网络在STS方面运行速度上的提升。

提取句向量后，使用像是cosine-similarity 或Manhatten/Euclidean distance可以找到语义上相似的句子

## Model

SBERT adds a pooling operation to the output of BERT/RoBERTa to derive a fixed sized sentence embedding.

实验了三种池化策略

1. Using the output of the CLS-token
2. Coputing the mean of all output vectors(MEAN-strategy)
3. Computin a max-over-time of the output vectors(MAX-strategy)

并在实验中使用多种目标函数

### Classification Objective Function

Concatenate the sentence embeddings u and v with the element-wise difference |u-v| and multiply it with the trainable weight后再接个Sofemax作为分类。

### Regression Objective Function

The cosine-similarity between the two sentence embeddings u and v is computed.

Using mean-squared-error loss as the objective function.

### Triplet Objective Function

Given an anchor sentence a, a positive sentence p, and a negative sentence n, triplet loss tunes the network such that the distance between a and p is smaller than the distance between a and n. Mathematically， we minizmize the following loss function.