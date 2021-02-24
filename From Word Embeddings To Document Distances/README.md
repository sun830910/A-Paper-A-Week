# From Word Embeddings To Document Distances

## Abstract

提出Word Mover's Distance(WMD), 一种用来度量两个文本档案之间距离的度量函数。

以预训练词向量为基础。

WMD 可以用来衡量两个文本文档之间的不相似性。

## Introduction

问题：如何判别两个文档中内容的距离？

可以应用在新闻分类聚类、歌曲识别、多文档匹配。

最简单的方法是构建一个bag of words(BOW)或term frequency-inverse document frequency(TF-IDF)，但这两种方法较难准确衡量出两个文档之间的距离。

