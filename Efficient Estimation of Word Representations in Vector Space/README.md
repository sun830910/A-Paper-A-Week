# Efficient Estimation of Word Representations in Vector Space

## Abstract

提出两种新模型，可以用来计算词为单位的连续形式向量表示。

使用相似词任务来衡量表示效果。  

## Introduction

原先使用基于字典的索引的作为词的表示，难以衡量两个字之间的相似程度。

自然语言模型得到的分布式向量在表现上优于N-gram模型。  

预期两个意义相近的字在分布式表示时也应相近，因其在语句中使用的地方也相似。  

  ## Model Architecture

### NNLM

At the input layer, N previous words are encoded using 1-of-V coding, where V is size of the vocabulary.  

The input layer is then projected to a projection layer P that has dimensionality N*D, using a shared projection matrix.  

选用了N=10,P between 500 to 2000,while the hidden layer size H is typically 500 to 1000 units.

隐藏层可以被用来计算在字典中各个词的概率分布。  

本文提出的模型use hierarchical softmax where the vocabulary is represented as a Huffman binary tree. Huffman trees assign short binary codes to frequent words, and this further reduces the number of output units that need to be evaluated.



主要介绍了几种LM结构



# New Log-linear Models

介绍提出的两种新模型架构，以前面章节谈到的【多数的时间复杂度是由非线性隐藏层导致的】，这也是为何神经网络如此吸引人。

神经网络可以由简单的两步骤训练而成：

1. 连续词向量可以由简单模型学习
2. 以分布式向量为基础训练N-gram NNLM模型。



## Continuous Bag-of-Words Model

与NNLM相似，但移除了非线性隐藏层并让所有词向量共享一映射层（不只是一个映射矩阵而已），所以所有的字都映射到了相同的位置。

学习任务：根据上下文预测当前字是什么。



## Continuous Skip-gram Model

学习任务：根据当前字预测当前的上下文是什么。

we use each current word as an input to a log-linear classifier with continuous projection layer, and predict word within a certain range before and after the current word.