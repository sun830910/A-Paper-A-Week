# GloVe: Global Vectors for Word Representation

## Abstract

Its a new global log-bilinear regression model that combines the advantage s of the two major model families in the literature: global matrix factorization and local context window methods.

Its efficiently leverages statistical information by training only on the nonzero elements in a word-word co-occurrence matrix, rather than on the entire sparse matrix or on individual context windows in a large corpus.

## Introduction

两个学习词向量的主流模型路线：

1. Global matrix factorization methods: such as latent semantic analysis(LSA)
2. Local context window methods: such as the skip-gram model.

This paper propose a specific weighted least squares model that trains on global word-word co-occurrence counts and thus makes efficient use of statistics.

## Related Work

### Matrix Factorization Methods

利用全局特征的矩阵分解方法。  

使用因式分解的方式将大矩阵降维以生成一低维度的词向量，例如LSA是基于奇异值分解：该方法对term-document矩阵（矩阵的每个元素为tf-idf）进行奇异值分解分解，从而得到term的向量表示和document的向量表示，此处使用的tf-ifg主要还是term的全局统计特征。

### Shallow Window-Based Methods

利用局部上下文的方法。  

通过局部滑动窗口配合深度学习模型预测文本任务来学习词向量，包含了Skim-gram和CBOW两种方法

## The GloVe Model

统计词的共现矩阵是无监督方法获得语料主要信息的重要来源。    

GloVe因其获得了全量语料的统计信息而叫做Global Vector。    

GloVe模型结合了Related work中的两个获得特征的方法，使用了语料库的全局统计（overall statistics）特征，也使用了局部的上下文特征（即滑动窗口），为此，GloVe模型引入了Co- occurrence Probabilities Matrix。  

---

首先引入词与词的共现矩阵。  

通过共现矩阵计算一可以反映词之间的相关性的Ratio值。  

GloVe模型的目标就是获取每一个词的向量表示，假设获得了词向量，则获得的词向量通过某种函数的作用后应呈现与Ratio一致的规律。

