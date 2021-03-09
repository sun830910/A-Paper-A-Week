# GloVe: Global Vectors for Word Representation

## Abstract

Its a new global log-bilinear regression model that combines the advantage s of the two major model families in the literature: global matrix factorization and local context window methods.

Its efficiently leverages statistical information by training only on the nonzero elements in a word-word co-occurrence matrix, rather than on the entire sparse matrix or on individual context windows in a large corpus.

## Introduction

两个学习词向量的主流模型路线：

1. Global matrix factorization methods: such as latent semantic analysis(LSA)
2. Local context window methods: such as the skip-gram model.

This paper propose a specific weighted least squares model that trains on global word-word co-occurrence counts and thus makes efficient use of statistics.



